---
title: "Provisioning files in a feature in the root directory"
date: "2012-11-30"
categories: 
  - "sharepoint-2010"
  - "visual-studio"
---

I was recently tasked with migrating code that was written for 2007  in a feature. The files that were originally provisioned in the feature we deployed to the root folder of the feature. The code had already been upgraded to SharePoint 2010. I needed to create a wsp using Visual Studio 2010 and package it as a feature. Out of the box when you create a new Module in VS 2010 you get a subfolder so for example you create the feature called Feature1 and a module called Mod1. The default deployment path is 14\\templates\\features\\feature1\\mod1\\file.txt. What I needed was feature1\\mod1\\file.txt. I tried modifying all the attributes in the elements.xml file and was not able to get the files to the correct location.

 

In the end what i did was the following

1. Add the file to the module in visual studio
2. Look at the properties of the file in visual studio
3. Under Deployment Location expand the + and clear the Path value.
4. Ensure that the Deployment Type is Element File
5. Deploy the file and it should be in the correct location.
