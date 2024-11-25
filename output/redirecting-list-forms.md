---
title: "Redirecting List Forms"
date: "2023-08-16"
categories: 
  - "m365"
  - "sharepoint-online"
coverImage: "adobestock_20902025.jpeg"
---

I recently encountered a situation with a client where I was working on setting up a list for a business process. The process required users to access a dedicated site to fill out a form, and we decided to embed the new form directly onto the site's homepage. Additionally, we wanted to display a list of all the submitted items by users.

[![](https://spdcp.com/wp-content/uploads/2023/08/formembeded.png?w=1024)](https://spdcp.com/wp-content/uploads/2023/08/formembeded.png)

However, a challenge arose when, upon submitting the form, it would redirect to the default view of the list. This resulted in duplicated content on the page and was far from an optimal user experience.

[![](https://spdcp.com/wp-content/uploads/2023/08/aftersubmit.png?w=1024)](https://spdcp.com/wp-content/uploads/2023/08/aftersubmit.png)

To address this issue, I found myself revisiting some techniques from the classic days of SharePoint to remember how query string parameters could be used to manipulate URLs.

Assuming you already have your list set up, here's a step-by-step guide to redirecting the forms:

1. **Create a SharePoint Page:** First, create a dedicated SharePoint page that will serve as a "thank you" page. This page will be the destination where the form will redirect after submission.

[![](https://spdcp.com/wp-content/uploads/2023/08/thankyou.png?w=1024)](https://spdcp.com/wp-content/uploads/2023/08/thankyou.png)

2. **Embed Web Part:** On the specific site page where you intend to display the form, add an embed web part. You can use the following syntax for redirection:  
      
    Use the following snippets to embed the new or edit forms on a SharePoint page.

```xml
<iframe src="https://{tenantname}.sharepoint.com/sites/{siteURL}/Lists/{listurl}/newform.aspx?Source=https://{tenantname}.sharepoint.com/sites/{siteURL}/SitePages/{thankyoupageurl}.aspx" />
```

```xml
<iframe src="https://{tenantname}.sharepoint.com/sites/{siteURL}/Lists/{listurl}/editform.aspx?Source=https://{tenantname}.sharepoint.com/sites/{siteURL}/SitePages/{thankyoupageurl}.aspx" />
```

In both cases, add the "Source" parameter to the query string. This parameter specifies the URL to which the form should redirect after submission. You'll notice that after adding the "Source" parameter, once a user submits the form, they will be redirected to the URL specified in the "Source" parameter

This redirection approach works whether you enter the URL in the Embed web part or input it as an IFrame.

[![](https://spdcp.com/wp-content/uploads/2023/08/thankyousubmitted.png?w=1024)](https://spdcp.com/wp-content/uploads/2023/08/thankyousubmitted.png)

By following these steps and incorporating the redirection URLs, you can significantly enhance the user experience when dealing with form submissions on SharePoint lists.
