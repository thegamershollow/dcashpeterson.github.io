---
title: "Custom Search MasterPage for SharePoint 2010"
date: "2012-08-07"
categories: 
  - "branding"
tags: 
  - "branding"
  - "search"
  - "sharepoint"
  - "sharepoint-2010"
---

I wish I could say that I was smart enough to have figured this one out for myself but I can't. I was working on my first custom branded SharePoint 2010 site and the search screen was all messed up. The navigation was displaying doubled. It turns out that the search center out of the box used minimal.master. This uses the placeholders that are in v4.master in different way so the search box ends up in the breadcrumb pull down. At any rate, [Randy Drisgill](http://blog.drisgill.com/2010/09/converting-custom-sharepoint-2010.html) wrote a great and very easy to follow post about how to modify your custom masterpage to the search center. It took me longer to create the feature to deploy the new search masterpage than it did to make the modifications.

 

Update: Randy Drisgill has changed his approach to branding the search screens but here is his original text below.

 

This is a topic that I’ve been asked probably a hundred times since the SharePoint 2010 bits were released. Before I get into how I solve it, I’ll provide a little back story. To quote from my upcoming book, [Professional SharePoint 2010 Branding & UI Design](https://web.archive.org/web/20121201081602/http://amzn.to/bHau4J):

> There are three site templates to choose from when creating search sites in SharePoint 2010: Enterprise Search Center, Basic Search Center, and FAST Search Center. Unlike most other sites in SharePoint 2010, these sites do not have v4.master or even nightandday .master applied to them; instead, they have minimal.master applied to them. If you have a custom master page that is based on v4.master, nightandday.master, or even one of the typical starter master pages and apply it to one of these search center sites, you notice that the search center doesn’t work properly. This is because the page layouts and pages that are created by default for these search center sites are hard coded specifically to work best with the way minimal.master is coded.

In fact, if you apply v4.master to one of the Search Centers, the search box seems to be removed from the page… when actually it is still on the page, just hiding up in the pop-out breadcrumb menu: [![image](https://web.archive.org/web/20121201081602im_/http://lh5.ggpht.com/_aV40Jy5RzE8/TKJaueH1itI/AAAAAAAAAdM/bqRbv9OHn20/image_thumb1.png?imgmax=800 "image")](https://web.archive.org/web/20121201081602/http://lh6.ggpht.com/_aV40Jy5RzE8/TKJauJnLe9I/AAAAAAAAAdI/siJr3PaG_lA/s1600-h/image3.png) As you can tell, this is less than ideal. Also problematic is the fact that the default minimal.master that is applied to the search centers doesn’t really contain traditional navigation or ways of navigating back to the parent site. So how do you get custom branding to work with the search center in SharePoint 2010? You have some options:

1. Create a new custom master page based on minimal.master
2. Adjust the page layouts or pages in the search center, to use the standard content placeholders (UPDATE: Here is a nice sandbox solution to help do this for you. In many cases this might be the best course of action if you are comfortable using 3rd party solutions: [http://sp2010searchadapters.codeplex.com/](https://web.archive.org/web/20121201081602/http://sp2010searchadapters.codeplex.com/ "http://sp2010searchadapters.codeplex.com/") )
3. Create a new custom master page with some minor adjustments for the way search centers work

For this post, we will focus on the third option. For simplicity, I will walk you through actually converting v4.master to work for the search centers. This can be useful if you want to show the typical SharePoint2010 navigation and UI that is normally hidden for the search center. The concepts here would work just as well with your own custom master page, in fact I have tried it a couple times with my own. Also, I will assume you understand the basics of working with master pages in SharePoint Designer 2010:

1. Make a copy of v4.master (or whatever other custom master page you are working with) and give it a new name like v4\_searchcenter.master.
2. Edit the new master page and locate and remove the PlaceHolderTitleBreadcrumb. This will allow the pop-out breadcrumb to still function properly:
    
    > <asp:ContentPlaceHolder id="PlaceHolderTitleBreadcrumb" runat="server"> <SharePoint:ListSiteMapPath runat="server" SiteMapProviders="SPSiteMapProvider,SPContentMapProvider" RenderCurrentNodeAsLink="false" PathSeparator="" CssClass="s4-breadcrumb" NodeStyle-CssClass="s4-breadcrumbNode" CurrentNodeStyle-CssClass="s4-breadcrumbCurrentNode" RootNodeStyle-CssClass="s4-breadcrumbRootNode" NodeImageOffsetX=0 NodeImageOffsetY=353 NodeImageWidth=16 NodeImageHeight=16 NodeImageUrl="/\_layouts/images/fgimg.png" RTLNodeImageOffsetX=0 RTLNodeImageOffsetY=376 RTLNodeImageWidth=16 RTLNodeImageHeight=16 RTLNodeImageUrl="/\_layouts/images/fgimg.png" HideInteriorRootNodes="true" SkipLinkText="" /> </asp:ContentPlaceHolder>
    
3. Next, add the PlaceHolderTitleBreadcrumb back right before the PlaceHolderMain. This will allow the search center to inject the search box in a good location:
    
    > <asp:ContentPlaceHolder id="PlaceHolderTitleBreadcrumb" runat="server"></asp:ContentPlaceHolder> <asp:ContentPlaceHolder id="PlaceHolderMain" runat="server"/>
    
4. Move PlaceHolderPageTitleInTitleArea (and any supporting HTML) to a hidden panel because this placeholder isn’t used the same way in the search center:
    
    > <asp:ContentPlaceHolder id="PlaceHolderPageTitleInTitleArea" runat="server" /> **<asp:Panel visible="false" runat="server">** **<asp:ContentPlaceHolder id="PlaceHolderPageTitleInTitleArea" runat="server" />** **</asp:Panel>**
    
5. For v4.master you will also want to remove ClusteredDirectionalSeparatorArrow and <h2></h2>. It won’t make sense to show these at the top now:
    
    > <SharePoint:ClusteredDirectionalSeparatorArrow runat="server"/> <h2></h2>
    
6. Next, several lines of CSS need to be added to make sure things look right for the search center. You can add them to the <head> section of the master page:
    
    > <style type="text/css"> /\* remove left margin \*/ .s4-ca { margin-left: 0px; } /\* remove gray background at top (optional) \*/ .srch-sb-results { background:transparent none repeat scroll 0 0; } /\* clean up top padding on 1st search page \*/ .srch-sb-main { padding-top: 20px; } /\* remove centering on 1st search page (optional) \*/ .srch-sb-results4 { margin: inherit; padding-left: 20px; } /\* remove background color on 1st search page (useful for colored designs) \*/ .ms-bodyareaframe { background-color: transparent; } /\* ------------------------------------------ \*/ /\* -- CSS that may be req. to reset the search styling -- \*/ /\* ------------------------------------------ \*/ /\* fix height of area above search results \*/ td.ms-titleareaframe, div.ms-titleareaframe, .ms-pagetitleareaframe { height: auto !important; } /\* fix border color on search results \*/ .ms-main .ms-ptabrx, .ms-main .ms-sctabrx, .ms-main .ms-ptabcn, .ms-main .ms-sctabcn { border-color: #eeeeee; } /\* fix arrangement of body area on search results \*/ .srch-sb-results { height: auto; } /\* fix positioning of prefs and advanced link on results \*/ .ms-sblink { display:block; } /\* fix the color of the prefs and advanced link on results \*/ .ms-sblink a:link, .ms-sblink a:visited, .ms-sblink a:hover { color:#0072BC; } </style>
    
    5\. Save the new master page, and check in / publish as a major version and approve it. Apply this master page to only the search center, and apply it as site master page while leaving system master page set as v4.master.

UPDATE: Some commenters have pointed out that if you want to edit pages in the Search Center (and who doesn’t?) these changes result in a double ribbon scenario. I haven’t fully tested this fix, so I’d be curious to hear about the results, but it looks like this would fix it:

1. Add <asp:ContentPlaceHolder id="PlaceHolderGlobalNavigation" runat="server"/> to the hidden panel.
2. Remove <div id="s4-ribboncont">…</div> and ALL of its contents.
3. Add in its place this line: <asp:ContentPlaceHolder ID="SPNavigation" runat="server"></asp:ContentPlaceHolder>

This should fix the double ribbon issue, but you also LOSE the top popout breadcrumb and the quick edit/save button at the top left of the ribbon.

Here are screenshots of the final result: [![image](https://web.archive.org/web/20121201081602im_/http://lh6.ggpht.com/_aV40Jy5RzE8/TKJaveW-kiI/AAAAAAAAAdU/V_iSpVbt4sY/image_thumb15.png?imgmax=800 "image")](https://web.archive.org/web/20121201081602/http://lh5.ggpht.com/_aV40Jy5RzE8/TKJau647v2I/AAAAAAAAAdQ/rNBbJZ7o64c/s1600-h/image14.png) [![image](https://web.archive.org/web/20121201081602im_/http://lh6.ggpht.com/_aV40Jy5RzE8/TKJawL4bWdI/AAAAAAAAAdc/Mc4TCTjkXBA/image_thumb17.png?imgmax=800 "image")](https://web.archive.org/web/20121201081602/http://lh4.ggpht.com/_aV40Jy5RzE8/TKJav0eiSGI/AAAAAAAAAdY/yUOA8L5h2V8/s1600-h/image16.png) You can employ these same techniques on your own master pages to create search center specific branding.

If you had trouble following the code changes you can download the completed v4 search center master page: [v4\_SearchCenter.zip](https://web.archive.org/web/20121201081602/http://bit.ly/rd-code)
