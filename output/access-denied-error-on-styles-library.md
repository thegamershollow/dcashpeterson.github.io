---
title: "Access Denied Error on Styles Library"
date: "2020-03-26"
categories: 
  - "o365"
  - "powershell"
  - "tips"
coverImage: "tips-and-tricks-1.jpeg"
---

I am going to file this one under the "if you need to do something more than twice you should probably write it down". This gets me frequently enough that I remember I've dealt with it before but not frequently enough that I can commit the solution to memory. I find myself using the SPFX React Menu extension quite a bit as a base for creating custom navigation controls. The navigation on that extension is powered by a json file that is uploaded to the Styles Library. In an OOTB site I whenever I try to upload the json file to the Styles Library I get an access denied error. Other traditional files like .txt or .docx work just fine.

Here is the PowerShell that I use to allow custom scripts. I got it from [Antti Koskella](https://www.koskila.net/how-to-enable-custom-scripts-for-a-sharepoint-online-site-collection/).

```powershell
# If you don't already have the modules, run Install-module first!
Import-Module -Name SharePointPnPPowerShellOnline -DisableNameChecking
 $adminUrl = "https://mytenant-admin.sharepoint.com/"
$siteurl = "https://mytenant.sharepoint.com/sites/MySiteUrl"

Connect-PnPOnline -Url $adminUrl -Credentials (Get-Credential)
 
$DenyAddAndCustomizePagesStatusEnum = [Microsoft.Online.SharePoint.TenantAdministration.DenyAddAndCustomizePagesStatus]
 
$context = Get-PnPContext
$site = Get-PnPTenantSite -Detailed -Url $siteurl
 
$site.DenyAddAndCustomizePages = $DenyAddAndCustomizePagesStatusEnum::Disabled
 
$site.Update()
$context.ExecuteQuery()
 
Disconnect-PnPOnline
```
