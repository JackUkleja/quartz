---
title: SQL Server 2008 Express with Advanced Services does *NOT* include Service Pack 1 (SP1)
date: 2009-09-10
layout: post
categories:
- Uncategorized
tags: []
comments: true
aliases:
    - ../sql-server-2008-express-with-advanced-services-does-not-include-service-pack-1-sp1
---

This is a very short post just to say that I spent 10 minutes trying to track down the answer to the question: **Does SQL Server 2008 Express with Advanced Services include Service Pack 1?**

It’s strange but I could find no blog posts that focused directly on this question, the official download page does not mention it, and I did not want to have to download every SQL Express distribution to find out.

Eventually I did find a post that revealed the answer, entitled “[Servicing SQL Server 2008 Express](http://blogs.msdn.com/petersad/archive/2009/02/25/servicing-sql-server-2008-express.aspx)”. Here is the key quote:
<blockquote>Starting with SQL Server 2008, we won’t be shipping new packages of "Express with Advanced Services" or "Express with Tools" with Service Pack 1.  To update these editions, you will need to run the full <a href="http://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=66ab3dbb-bf3e-4f46-9559-ccc6a4f9dc19">Service Pack 1</a>.</blockquote>
So the answer is a pretty clear **No** – the SQL Server 2008 Express with Advanced Serviced/Tools packages **do not include service pack 1**. And by the way, the full service pack 1 mentioned in the quote is approx 300 MB.

I have updated my [SQL Server 2008 Express direct download table](http://jack.ukleja.com/direct-download-links-for-sql-server-2008-express/) to include whether or not the download includes SP1. In summary only the “runtime” download includes SP1… the rest you will have to patch manually.

Don’t know about you, but I’m mildly annoyed by that conclusion.