---
title: d:DesignInstance in Depth
date: 2010-03-05
layout: post
categories:
- wpf
tags: []
comments: true
aliases:
    - ../ddesigninstance-in-depth/
---

_Update: I have contacted WPF "User education" team at Microsoft about the lack of documentation. The response was:_ 

> "I've looked into this and there was some confusion around whether or not these were publicly exposed. Thanks for letting us know about this error **We are now working on getting these documented**."

There is currently no formal documentation for the `DesignInstanceExtension` (which you will see in XAML as `d:DesignInstance`).

The best two articles are by [Unni](http://blogs.msdn.com/unnir/archive/2009/07/12/introducing-sample-data-for-developers.aspx) and [Karl](http://karlshifflett.wordpress.com/2009/10/28/ddesigninstance-ddesigndata-in-visual-studio-2010-beta2/) but there are a few things they don’t cover.

This is what I have worked out by trial and error:

According to Intellisense DesignInstance has got 3 parameters: `Type`, `IsDesignTimeCreatable` and `CreateList`.

<table border="0" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td width="400" valign="top"><strong>Type</strong></td>
<td width="400" valign="top">This is the type of the object for which the design-time data binding “shape” will be derived</td>
</tr>
<tr>
<td width="400" valign="top"><strong>IsDesignTimeCreatable</strong></td>
<td width="400" valign="top">(Defaults to false)

If this is <strong>true</strong> then a new instance of your type will be created at designtime. If you want <em>data</em> to actually appear in the designer you will need to make sure suitable properties are being set in your constructor. The best way to do this is to create a derived type of your model type, where you are free to set properties to default values without polluting your real types. If your model type implements an interface even better – you can create a design time model that way.

If this is <strong>false</strong> VS/Blend will use reflection to create a dummy type with the same shape as your specified <strong>Type</strong>. The problem with this is the dummy type will have no instance data in it thus you will not see anything in the designer. When this is <strong>false</strong> you are really only getting the benefit of assisted type-aware data binding. (If you need lots of sample data you are better off using the <strong>DesignData</strong> extension)</td>
</tr>
<tr>
<td width="400" valign="top"><strong>CreateList</strong></td>
<td width="400" valign="top">(Defaults to false)

If <strong>true</strong> the <strong>DesignInstance</strong> will actually be created as a list of the specified <strong>Type</strong>. From what I can tell this is the only way to specify your design instance as a collection (I briefly tried to pass a generic list as the <strong>Type</strong> parameter but could not get it to work, hence why I expect this parameter exists).</td>
</tr>
</tbody></table>
Note that DesignInstance and DesignData are mutually exclusive mark-up extensions. You cannot set both DesignInstance and DesignData extensions when setting your DataContext.

In general the main use of DesignInstance extension is to allow you to use the data binding tools within Blend and Visual Studio. It can give you some sample data if you set properties in your constructor.

To be able to design effectively you really need plentiful and varied sample data. For the best design experience you should consider using the DesignData extension – the drawbacks being the slightly increased upfront cost of creating a XAML file containing your sample data, and the increased complexity of refactoring the XAML whenever your classes change.