---
title: WCF Data Services vs WCF RIA Services
date: 2009-12-02
layout: post
categories:
- Uncategorized
tags: []
comments: true
aliases:
    - ../wcf-data-services-vs-wcf-ria-services/
---

I’ve been having trouble finding a straight forward comparison of these two technologies. On the surface they appear to be solving similar, if not the same problems.
  
Make no mistake – the message coming from Microsoft is not clear or consistent – which probably explains the confusion. I suspect the relationship between these two products is still being “discovered” by MS.
  
Anyway, in this [bliki](http://en.wikipedia.org/wiki/Bliki) I will try to sum up my findings as to the differences between them as I figure them out.
  <table border="0" cellspacing="0" cellpadding="2" width="800"><tbody>     <tr>       <td valign="top" width="400">         <h3>WCF (ADO.NET) Data Services</h3>       </td>        <td valign="top" width="400">         <h3>WCF (.NET) RIA Services</h3>       </td>     </tr>      <tr>       <td valign="top" width="400">Expose data model as RESTful web service</td>        <td valign="top" width="400">Prescriptive approach to n-tier app development</td>     </tr>      <tr>       <td valign="top" width="400">Cross platform interoperation as a goal          <br>- “Unlock data silos”           <br>- Out-of-box support from future MS products such as SQL2008 R2, Azure, Excel 2010, SharePoint 2010</td>        <td valign="top" width="400">Designed specifically for end-to-end Silverlight &amp; ASP.NET solutions          <br>- Some technology proprietary to Silverlight (no WPF support)           <br>- Use ASP.NET Authentication/Roles across SL and ASP.NET           <br>- ASP.NET/AJAX can also access service layer</td>     </tr>      <tr>       <td valign="top" width="400">Loosely coupled clients and servers</td>        <td valign="top" width="400">Client &amp; server are designed and deployed together</td>     </tr>      <tr>       <td valign="top" width="400">Service layer exposes “raw” data sources</td>        <td valign="top" width="400">Opportunity to easily add business logic into service layer          <br>- Encourage “domain” concepts           <br>- Strong validation framework           <br>- Offline / Sync enabled</td>     </tr>      <tr>       <td valign="top" width="400">Service can be consumed from .NET, Silverlight, AJAX, PHP and Java (libraries available)</td>        <td valign="top" width="400">Service can be consumed easily from SL, AJAX, WebForms          <br></td>     </tr>      <tr>       <td valign="top" width="400">Service’s data source must:          <br>- Expose at least one IQueryable property           <br>- Implement IUpdateable if you desire updates</td>        <td valign="top" width="400">Service exposes domain objects via convention:          <br>- IQueryable GetX           <br>- UpdateX/InsertX/DeleteX</td>     </tr>      <tr>       <td valign="top" width="400">No design time experience yet (??)</td>        <td valign="top" width="400">Design time experience with data sources, drag drop etc</td>     </tr>      <tr>       <td valign="top" width="400">- OData for all clients          <br>- Within OData, multiple formats supported (JSON, XML etc)</td>        <td valign="top" width="400">- SOAP (binary) for SL clients         <br>- JSON for AJAX clients          <br>- SOAP (XML) for other clients          <br></td>     </tr>      <tr>       <td valign="top" width="400">Discoverable (?)</td>        <td valign="top" width="400">Non-discoverable</td>     </tr>      <tr>       <td valign="top" width="400">Hosted as WCF Service (.svc)</td>        <td valign="top" width="400">Old version hosted in custom web handler (.axd).          <br>New version is WCF service.</td>     </tr>      <tr>       <td valign="top" width="400">Standardized on <a href="http://www.odata.org/">OData</a> protocol </td>        <td valign="top" width="400">Will “support” OData</td>     </tr>      <tr>       <td valign="top" width="400">More mature – public for at least 2 years, formerly “Project Astoria”</td>        <td valign="top" width="400">Less mature - public for 6 months</td>     </tr>   </tbody></table> 
  
[ad#Google Adsense #1] 

### Common features

* Based on WCF     
* Use a RESTful architecture     
* Can be used to expose any data source (sql, xml, poco/objects etc.)    
* Client side libraries provide ability to query using LINQ 
  
### General
   
* Currently they do not share much (any?) technology / code     
* RIA Services is not based on top of Data Services     
* RIA Services &amp; Data Services will “align”    
* OData eventually pushed down into WCF stack  
  
Your opinions are welcome!
  
### References

http://blogs.msdn.com/brada/archive/2009/03/19/what-is-net-ria-services.aspx
  
http://mschannel9.vo.msecnd.net/o9/mix/09/pptx/t36f.pptx
  
http://blogs.msdn.com/endpoint/archive/2009/11/18/the-wcf-services-ecosystem.aspx
  
http://www.douglaspurdy.com/2009/11/20/on-odata-open-data-protocol/
  
http://msdn.microsoft.com/en-us/data/ee844254.aspx
  
http://blogs.msdn.com/saurabh/archive/2009/11/23/understanding-the-wcf-in-wcf-ria-services.aspx
