---
title: "Themes for your SharePoint 2010 environment"
date: "2012-08-07"
categories: 
  - "branding"
tags: 
  - "branding"
  - "sharepoint"
  - "sharepoint-2010"
  - "themes"
---

I spend most of my time working on custom branding projects using SharePoint. These are great projects and tend to be large custom dev projects. They also tend to have larger budgets. But what about those clients that want a different look and feel but don't have the large budget. Use a custom theme or template. There are several packaged themes out there for SharePoint 2010 that provide you with a look and feel that while not unique does change the way that SharePoint looks. The themes also provide some level of customization either during the purchase process or after. One of the key things to take into account is that there is probably going to be some level of customization to be done after the fact.

[SharePoint Packages](http://www.sharepointpackages.com/): SharePoint Packages offers 5 different SharePoint 2010 themes. I have implimented these for several clients with great results. If you get the platnium package they work with all the OOB SharePoint site templates including MySites. The underlying CSS is not difficult to modify. There are two things to keep in mind with using these themes. Changes made to the theme are not reflected untill you do an IISreset and unapply the theme and reapply the theme to the site so make sure to do all your changes in dev and then put them out on production. The last is that the install process is manuall. You need to copy the files to the 14 hive and import them into the MasterPage gallery. Because of this while it does support branding MySites it is cumbersome for Personal sites. To truly automate this you would need to take all the branding files and include them in a WSP and use feature stapling. Not a big deal but requires a developer to do this.

[PixelMill](http://www.pixelmill.com/): I was at the SharePoint Conference 2011 and I went to the booth for these folks. It looks like they have interesting products. I have not implimented them yet. They do not support MySites according to their web site. It does look like it is easy to update the CSS after the fact.

[SharePoint MasterPages](http://sharepointmasterpages.com/): These people have been around for a while. They have a robust offering of SharePoint 2007 themes and new offerings for 2010. In addition, they also claim that their templates are ready for Office 365 which none of the others point out.

[Bind Tuning](http://sharepoint.tuning.bind.pt/): This site has several layouts, that feel less like SharePoint, and more like a custom branded site. You also have the ability to use a WYSIWYG editor to customize the OOB to get closer to your individual brand. The site does not list the SharePoint site templates that it supports. I have used these themes and while the price point is great and the customization is excellent it does not support feature stapling so if you want the theme to be applied automatically you will need to create a feature staple feature. The files for this theme do get compiled into a WSP so that does make deployment across the farm easier.

One thing that I like to caution people about when I am recomending a theme is make sure that the theme you choose is close to what you want the end result to look at. It doesn't make a lot of sense for you to select a theme that uses rounded corners and gradients if you don't like rounded corners and gradients. It will be lots of pain and angst to try to change a theme that significantly.
