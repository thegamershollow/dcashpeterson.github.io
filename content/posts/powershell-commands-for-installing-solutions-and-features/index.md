---
title: "Powershell Commands for Installing Solutions and Features"
date: "2012-08-07"
categories: 
  - "powershell"
tags: 
  - "powershell"
  - "sharepoint"
  - "sharepoint-2010"
---

As a SharePoint 2007 developer I have many of the common stsadm commands committed to memory. While stsadm still works in SharePoint 2010 in favor of moving to proficiency in the latest version of the technology here are some of the powershell commands that I use on a daily basis. With that said, I usually create a batch file that runs these commands so that I only need to run one command to get the features updated and features activated.

**Add Solution**  
Add-SPSolution c:\\solutions\\myproject.wsp

**Deploy Solution**  
Install-SPSolution –Identity myproject.wsp –WebApplication http://myprojectsite -GACDeployment  
Note: You can also use the following parameters

- –CASPolicies: If you are not deploying to the GAC
- –AllWebApplications: if you want to deploy the solution to all web applications
- –Force: to force the deployment of the solution

**Upgrade Solution**  
Update-SPSolution –Identity myproject.wsp –LiteralPath c:\\solutions\\myproject.wsp –GACDeployment

**Retract Solution**  
Uninstall-SPSolution –Identity myproject.wsp –WebApplication http://myprojectsite

**Remove Solution**  
Remove-SPSolution –Identity myproject.wsp

**Activate Feature**  
Enable-SPFeature –Identity MyFeature –url http://myprojectsite

**Deactivate Feature**  
Disable-SPFeature –Identity MyFeature –url http://myprojectsite
