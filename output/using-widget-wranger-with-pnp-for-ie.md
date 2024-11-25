---
title: "Using Widget Wrangler with PnP for IE"
date: "2018-01-17"
categories: 
  - "featured"
  - "pnp"
  - "sharepoint-2013"
coverImage: "pexels-photo-414579.jpeg"
---

I am creating client-side webparts using the [Widget Wrangler](https://github.com/Widget-Wrangler/ww) for SharePoint. The webparts all use PnP JS. This site needs to work in IE 10+. The webparts work great in Chrome and most other browsers. However, when i run it in IE I get an error about Undefined Headers. This is because PnP uses Promises which IE 10 and 11 don't support. To fix that I had to include some polyfills and perform a sacrifice to the SharePoint gods.

1. In the ww-appScripts property I needed to add the following references
    1. https://cdnjs.cloudflare.com/ajax/libs/fetch/2.0.3/fetch.min.js
    2. https://cdnjs.cloudflare.com/ajax/libs/es6-promise/4.1.0/es6-promise.min.js
    3. https://cdnjs.cloudflare.com/ajax/libs/sp-pnp-js/2.0.5/pnp.min.js

So the tag ends up looking like this

{"src": "https://ajax.googleapis.com/ajax/libs/angularjs/1.4.5/angular.js", "priority":0},  
{"src": "https://code.jquery.com/jquery-3.2.1.min.js", "priority": 1},  
{"src": "https://code.jquery.com/ui/1.12.1/jquery-ui.min.js", "priority": 2},  
{"src": "https://cdnjs.cloudflare.com/ajax/libs/select2/4.0.4/js/select2.min.js", "priority": 2},  
{"src": "https://cdnjs.cloudflare.com/ajax/libs/fetch/2.0.3/fetch.min.js", "priority": 3},  
{"src": "https://cdnjs.cloudflare.com/ajax/libs/es6-promise/4.1.0/es6-promise.min.js", "priority": 4},  
{"src": "https://cdnjs.cloudflare.com/ajax/libs/sp-pnp-js/3.0.1/pnp.min.js", "priority": 5},  
{"src": "~/bundle.js", "priority":6}

Important thing to note that the priority Fetch and ES6-Promise need to come before PnP.

Also in your PnP setup make sure to include

$pnp.setup({  
headers: {  
"Accept": "application/json; odata=verbose"  
}  
});
