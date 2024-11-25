---
title: "Site Design for Easy Site Page Management"
date: "2020-04-16"
categories: 
  - "o365"
  - "powershell"
  - "sharepoint-online"
  - "site-designs"
  - "site-scripts"
coverImage: "adobestock_116134019.jpeg"
---

Does your site pages library look totally unmanageable? Have you had instances where you can see a news post that you created but someone else doesn't? Is your content not showing up in search or highlighted content web parts? This happens all the time and it is generally an easy fix. I had the good fortune to work with Susan Hanley on a project recently. We were discussing creating a site design to automate some of the pieces of the site architecture she created. She mentioned to me a blog post she created, [One simple view to improve SharePoint page and site management](https://www.computerworld.com/article/3441838/one-simple-view-to-improve-sharepoint-page-and-site-management.html). In that post she proposes making minor modifications to the All Pages view to help alleviate some of these common issues and make it easier to manage site pages. She challenged me to create a site script that would replace or update the existing view to include the fields like promoted state to help better manage the contents of the site pages library.

The first thing I did was create a new site and followed the instructions in Sue's post, adding Promoted State, Checked Out To, and removing Created and Created By. Once I had it the way I wanted it I exported the list as a site script.

```
$adminSiteUrl = "https://mytenant-admin.sharepoint.com/"
$listUrl = "https://mytenant.sharepoint.com/sites/samplesite/SitePages"

#Connect to SharePoint using Stored Credentials
$creds = Get-PnPStoredCredential -Name O365DevTenant -Type PSCredential
Connect-SPOService $adminSiteUrl -Credential $creds

#Get the site script for the list
$extracted = Get-SPOSiteScriptFromList -ListUrl $listUrl

#Write this to a JSON file
$scriptFile = $PSScriptRoot + "\SitePagesLibrary.json"
Set-Content -Path $scriptFile -Value $extracted
```

That will get the JSON that defines the entire Site Pages list. This includes things that are already provisioned like the content types and the OOTB views. You can use that JSON as is or you can strip it down so it just has the updated All Pages view in it like this. .

```jscript
{
  "$schema": "https://developer.microsoft.com/json-schemas/sp/site-design-script-actions.schema.json",
  "actions": [
    {
      "verb": "createSPList",
      "listName": "Site Pages",
      "templateType": 119,
      "subactions": [
        {
          "verb": "addSPView",
          "name": "All Pages",
          "viewFields": [
            "DocIcon",
            "LinkFilename",
            "Editor",
            "Modified",
            "PromotedState",
            "CheckoutUser",
            "_UIVersionString"
          ],
          "query": "<OrderBy><FieldRef Name=\"Modified\" Ascending=\"FALSE\" /></OrderBy><GroupBy><FieldRef Name=\"PromotedState\" Ascending=\"TRUE\" /></GroupBy>",
          "rowLimit": 30,
          "isPaged": true,
          "makeDefault": true
        }
      ]
    }
  ]
}
```

When I first started working on this I was copying the content from the output from PowerShell ISE. That includes artificial line breaks so if you copy from the ISE window make sure you to remove all the line breaks so that each line is on it's own.

Once you have the site script created you need to add it to your tenant and create a new Site Design. You can do all of that pretty easily with PnP PowerShell. I am by no means a PowerShell expert but I know someone who is. When I was talking with [Todd Klindt](https://www.toddklindt.com/default.aspx) he introduced me on to using Credential Manager so I don't need to type in my credentials every time. [Check out his post on the subject](https://www.toddklindt.com/blog/Lists/Posts/Post.aspx?ID=800). It is super simple.

```powershell
$adminSiteUrl = "https://mytenant-admin.sharepoint.com/"

#Set values for Site Script
$title = "Site Pages Script"
$description = "Updated All Pages library to group by Promoted State"
$scriptFile = $PSScriptRoot + "\SitePagesLibrary.json"

#Set values for Site Design
$siteDesignTitle = "Site Pages Site Design";
$siteDesignDescription = "Site design that deploys changes to All Pages view to group by promoted state";

#Connect to SharePoint using Stored Credentials
$creds = Get-PnPStoredCredential -Name O365DevTenant -Type PSCredential
Connect-PnPOnline –Url $adminSiteUrl –Credentials $creds

#Add Site Script to tenant
$fileContents = Get-Content $scriptFile -Raw
$siteScript = Add-PnPSiteScript -Title $title -Description $description -Content $fileContents

$siteScript
#Create the Site Design
Add-PnPSiteDesign -Title $siteDesignTitle -SiteScriptIds $siteScript.Id -Description $siteDesignDescription -WebTemplate CommunicationSite
```

That should do it. Now when you create a new site you can see it in the list of Site Designs.

![](https://spdcp.com/wp-content/uploads/2020/04/site-design.jpg?w=1024)

If you navigate to your Site Pages library you should see the updated view with the sites grouped by promoted state. As Sue points out.

- Promoted State of 0 = Site Page
- Promoted State of 1 = Unpublished News Post
- Promoted State of 2 = Published News Post

![](https://spdcp.com/wp-content/uploads/2020/04/site-designs2.jpg?w=1024)

I hope this is helpful and now makes it easy to update the site pages library and make it easier to manage your news items and site pages.
