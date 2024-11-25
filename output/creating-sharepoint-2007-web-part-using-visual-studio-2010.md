---
title: "Creating SharePoint 2007 Web Part using Visual Studio 2010"
date: "2012-08-07"
tags: 
  - "moss"
  - "sharepoint-2007"
---

One of the things that I love about Visual Studio 2010 is the automated feature and solution creation. This works great when you are developing a web part for SharePoint 2010 and using Visual Studio 2010. Where I ran into a problem was I needed to create a web part for SharePoint 2007. It took me the better part of the day but I managed to get the web part working and deployed to a SharePoint 2007 farm.

I started by creating a blank SharePoint solution in Visual Studio 2010. After that I checked all the references included in the project. These all reference version 14.0 (SharePoint 2010). You will need to delete and add the references again pointing to the version 12.0 dlls. If you don’t have these on your workstation you can copy them from C:\\Program Files\\Common Files\\Microsoft Shared\\Web Server Extensions\\12\\ISAPI on a SharePoint 2007 server and paste them somewhere you will find them easily. I put them in a folder under C:\\Users\\<username>\\Documents\\Visual Studio 2010\\Projects\\dlls.

Open the Package.Package and click the designer. In the properties window look for the SharePoint Product Version number attribute and remove the value.

After this when you use the Visual Web Part template you will need to make a few modifications.  
1) Remove the reference to Microsoft.Web.CommandUI.  
2) Change all the reference versions from 14.0.0.0 to 12.0.0.0

Build the project and package the solution. You should now be able to deploy the wsp file normally.
