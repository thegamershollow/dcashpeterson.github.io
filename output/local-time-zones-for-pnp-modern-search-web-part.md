---
title: "Local Time Zones for PnP Modern Search Web Part"
date: "2019-12-21"
categories: 
  - "handlebars"
  - "o365"
  - "pnp"
coverImage: "adobestock_178600457.jpeg"
---

I was working with the [PnP Modern Search Web Part](https://microsoft-search.github.io/pnp-modern-search/) which is a great tool. If you have not seen it take a look. It provides a lot of robust features that allow you to aggregate content and display results in many different ways. I recently had an issue where I had created a custom Handlebars template to show the events. The layout worked great. The one issue was when the times for the dates were output they were in UTC.

What does this look like.

The list item has the Event Date set to 12/17/2019 11:00am (EST)  
  
In the Handlebars template I used {{EventDateOWSDate}}. This outputs 2019-12-17T16:00:00.0000000  
  
In checking the Search Query Tool the value is rendered as 2019-12-17T16:00:00.0000000Z

The [documentation](https://microsoft-search.github.io/pnp-modern-search/search-parts/templating/) for the web part indicates there are helper functions to help format dates and specifically specifies that you need the Z at the end.  
  

<table><tbody><tr><td><code>{{getDate &lt;date_managed_property&gt; "&lt;format&gt;" &lt;time handling&gt;}}</code></td><td>Format the date with Moment.js&nbsp;according to the current language. Date in the managed property should be on the form&nbsp;<code>2018-09-10T06:29:25.0000000Z</code>&nbsp;for the function to work.&lt;time handling&gt; is optional and takes<br><br>0 = format to browsers time zone (default)<br>1 = ignore Z time and handle as browsers local time zone<br>2 = strip time and set to 00:00:00 in browsers local time zone<br>3 = display in the time zone for the current web<br>4 = display in the time zone from the users profile</td></tr></tbody></table>

This uses momentJS to help process the dates. Unfortunately, when I tried to use the time handling options I realized that the value coming from {{EventDateOWSDate}} is missing a very important piece of information, the "Z" at the end. This "Z" indicates that this is UTC time. In other words it knows what time it is and knows the offset from UTC but doesn't know that it is actually in UTC time so it can't do any conversions to it.

In the end I was able to provide a "fix" for the issue by adding the Z to the end of my date string and formatting it correctly. I put "fix" in quotes because it doesn't really feel right to call the same function twice to get different results. If anyone knows of a better way to do this I am open to options.

```
{{ getDate (getDate EventDateOWSDATE "YYYY-MM-DDTHH:mm:ss.0000000\Z") "ddd, MMM DD h:mm a" 0 }}
```

The getDate in the parentheses formats the date in UTC format. the \\Z is the escape sequence. We can't just use Z because that is the time zone offset so it would output the date with -5:00 (EST is -5 hours from UTC). So adding the \\Z creates the string we need. 2019-12-17T16:00:00.0000000Z

The second getDate takes the string date and formats it in the format we want to output. In this instance Tuesday, Dec 17 11:00 am.

So while this isn't the most elegant solution it did fix my issue.
