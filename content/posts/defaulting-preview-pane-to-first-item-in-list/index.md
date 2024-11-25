---
title: "Defaulting Preview Pane To First Item in List"
date: "2012-08-07"
tags: 
  - "development"
  - "sharepoint"
  - "sharepoint-2010"
---

A client of ours wanted to use the preview pane for a list view. The default view of the preview pane has the right hand pane blank. You need to select an item or hover an item from the left pane to populate the right pane. The client wanted the right hand pane to have the right hand pane default to first item in the list.

UPDATE 10/21/2013: It looks like when people are doing a copy and paste of the code that the double quotes " are getting converted to smart quotes. When you copy and paste make sure that you change any of the single or double quotes to plain text.

UPDATE 9/11/2013:

- On the list view page (e.g. allitems.aspx) att ?ToolPaneView=2 to the query string. Replace ? with & if there are already properties in the query string.
- Under the page tab click edit page
- Add a content editor web part to the page above the list items web part.
- Click inside the content editor web part where is says add new content.
- From the ribbon select Edit HTML Source
- In the resulting pop up window paste the following code. <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.10.2/jquery.min.js"></script> <script> $(document).ready( function () {
    
              $('div.ms-ppleft table tr td.ms-vb-title').first().trigger('onfocus'); } ) </script>
    
- Stop editing the page.
- The preview pane should now default to the first item in the list.
