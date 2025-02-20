---
title: Diagnosing ASP.NET page compilation errors
date: 2013-02-19
layout: post
categories:
- Software Development
tags: []
comments: true
aliases:
    - ../diagnosing-asp-net-page-compilation-errors/
---

Page compilation errors are a common cause of 'yellow screens of death' (YSOD) encountered by ASP.NET developers. During the heat of development it's very easy to ignore the details of these error messages, and instead resolve them with a bit of intuition. But there are limits to intuition. Today I had to drill into one of these errors and I learnt quite a bit along the way.

## Are you missing an assembly reference?

I'm going to tackle some of the most common page compilation errors, which relate to missing references and/or incorrect using statements. The screenshot below shows a typical 'CS0234' error:

![](https://s3-us-west-2.amazonaws.com/jack-ukleja-com/ysod-asp-net-namespaces-missing-references.png)

The compiler error message reads:

> The type or namespace name 'Helpers' does not exist in the namespace 'System.Web' (are you missing an assembly reference?).

## Page compilation errors

To understand what is going on it's useful to have a quick reminder of how ASP.NET works under the hood.

Every page or view in an ASP.NET site ultimately gets converted into executable code. By default this happens at runtime.

It's a two-step process: Using a Razor view as an example, first the Razor view engine uses code generation to translate the .cshtml file into a .cs file. Then ASP.NET compiles that .cs source into an executable binary.

It's errors occuring during this second step which reveal themselves as YSODs like the one shown above. YSODs *do *actually include useful debug information...

## YSOD examined

Clicking 'Show Complete Compilation Source' shows me the full C# source file which has been generated by the view engine for the requested page. Here you can clearly see all the using statements generated by the view engine code generator.

Clicking on 'Show Detailed Compiler Output' shows you exactly how csc.exe (the C# compiler) was invoked by ASP.NET in order to compile the page's generated C# code. Importantly the /R: parameters are the referenced assemblies.

Every C# developer knows that in order to succesfully build an assembly from a module of code you need to make sure all the types you use are made available to the compiler via references.

With normal C# projects you add your own references and write your own using statements. But what determines the references and using statements when code is generated for pages and compiled by ASP.NET?

## References

The references used by ASP.NET during page compilation are specified entirely in configuration files using the [assemblies element for compilation](http://msdn.microsoft.com/en-us/library/vstudio/bfyb45k1(v=vs.100).aspx).

*"But I haven't specified an assemblies element!"*, I hear you say.

The reason you still get some by default is that the **root-level** web.config file (typically found in `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config`), that comes distributed with .NET, contains something akin to the following:

        <compilation>
            <assemblies>
                <remove assembly="Microsoft.VisualStudio.Web.PageInspector.Loader, Version=1.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" />
                <add assembly="Microsoft.VisualStudio.Web.PageInspector.Loader, Version=1.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" />
                <add assembly="mscorlib" />
                <add assembly="Microsoft.CSharp, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" />
                <add assembly="System, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
                <add assembly="System.Configuration, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" />
                <add assembly="System.Web, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" />
                <add assembly="System.Data, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
                <add assembly="System.Web.Services, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" />
                <add assembly="System.Xml, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
                <add assembly="System.Drawing, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" />
                <add assembly="System.EnterpriseServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" />
                <add assembly="System.IdentityModel, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
                <add assembly="System.Runtime.Serialization, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
                <add assembly="System.ServiceModel, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
                <add assembly="System.ServiceModel.Activation, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35" />
                <add assembly="System.ServiceModel.Web, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35" />
                <add assembly="System.Activities, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35" />
                <add assembly="System.ServiceModel.Activities, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35" />
                <add assembly="System.WorkflowServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35" />
                <add assembly="System.Core, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
                <add assembly="System.Web.Extensions, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35" />
                <add assembly="System.Data.DataSetExtensions, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
                <add assembly="System.Xml.Linq, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
                <add assembly="System.ComponentModel.DataAnnotations, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35" />
                <add assembly="System.Web.DynamicData, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35" />
                <add assembly="System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35" />
                <add assembly="*" />
                ...
            </assemblies>
        </compilation>
    
Unless you make configuration changes this list will match the referenced assemblies you observe 'Show Detailed Compiler Output' section of the YSOD. Excluding one important exception...

Note that the final entry is special. It uses a star '*' instead of providing an assembly name. The [remarks on MSDN](http://msdn.microsoft.com/en-us/library/vstudio/37e2zyhb(v=vs.100).aspx) explain it's purpose:

> Optionally, you can specify the asterisk (*) wildcard character to add every assembly within the private assembly cache for the application, which is located either in the \bin subdirectory of an application or in the.NET Framework installation directory (%systemroot%\Microsoft.NET\Framework\version).

The implications of this are that you can easily ensure additional assemblies get referenced during page compilation by placing them in your private assembly cache. And you do that simply by changing the "Copy Local" property of an assembly to true (correlated with the [Private](http://msdn.microsoft.com/en-us/library/bb629388.aspx) reference attribute within an MSBUILD file).

But what if you don't want to add the assembly to your private assembly cache? In that case you just explicity add it to your **application-level** web.config using the same `assemblies.add` element shown above.

## Namespaces

Where do the default using statements come from for the generated code? Well this actually depends on the view engine you are using. The default view engine (Web Forms) takes its defaults from the **root-level** web.config under the [namespaces element](http://msdn.microsoft.com/en-us/library/vstudio/ms164642(v=vs.100).aspx):

        <pages>
            <namespaces>
                <add namespace="System" />
                <add namespace="System.Collections" />
                <add namespace="System.Collections.Generic" />
                <add namespace="System.Collections.Specialized" />
                <add namespace="System.ComponentModel.DataAnnotations" />
                <add namespace="System.Configuration" />
                <add namespace="System.Linq" />
                <add namespace="System.Text" />
                <add namespace="System.Text.RegularExpressions" />
                <add namespace="System.Web" />
                <add namespace="System.Web.Caching" />
                <add namespace="System.Web.DynamicData" />
                <add namespace="System.Web.SessionState" />
                <add namespace="System.Web.Security" />
                <add namespace="System.Web.Profile" />
                <add namespace="System.Web.UI" />
                <add namespace="System.Web.UI.WebControls" />
                <add namespace="System.Web.UI.WebControls.WebParts" />
                <add namespace="System.Web.UI.HtmlControls" />
                <add namespace="System.Xml.Linq" />
            </namespaces>
            ...
        </page>
    

## Namespaces with Razor

If you are using Razor the [default namespaces are hardcoded](http://stackoverflow.com/questions/14845541/where-are-the-default-razor-cshtml-namespaces-defined)!

You can easily *add* to the list of default namespaces by adding a razor-specific namespaces element to your `web.config` (sample shown below), but you cannot easily *remove* default namespaces. Even attempting to use `<clear/>` does not work. If you really need to get rid of default namespaces you will have to do it in code.

    <system.web.webPages.razor>
      <host factoryType="System.Web.Mvc.MvcWebRazorHostFactory, System.Web.Mvc, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35" />
      <pages pageBaseType="System.Web.Mvc.WebViewPage">
       <namespaces>
        <add namespace="System.Web.Mvc" />
        <add namespace="System.Web.Mvc.Ajax" />
        <add namespace="System.Web.Mvc.Html" />
        <add namespace="System.Web.Optimization"/>
        <add namespace="System.Web.Routing" />
       </namespaces>
      </pages>
    </system.web.webPages.razor>
    
## A note about Web.configs

![](https://s3-us-west-2.amazonaws.com/jack-ukleja-com/views-web-config.png)

Bare in mind that projects often have a web.config specifically for the Views folder (e.g. MVC) and the page compiler will pay attention to the config heirarchy. Typically you will want to makes changes specific to your Views `web.config`.

## Conclusion

Using the information in this blog post you should be able to understand and resolve any page compilation errors relating to missing references and/or incorrect using statements. Good luck!