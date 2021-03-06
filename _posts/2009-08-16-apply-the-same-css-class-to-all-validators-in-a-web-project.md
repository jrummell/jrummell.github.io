---
permalink: /apply-the-same-css-class-to-all-validators-in-a-web-project
redirect_from: /apply-the-same-css-class-to-all-validators-in-a-web-project/
title: Apply the same CSS class to all validators in a web project 
layout: post
date: 2009-08-16 14:33:01
published: true
tags: css validation
---


I recently had to add a CSS class to all validators in an ASP.NET web application. I started with the theme’s [skin](http://msdn.microsoft.com/en-us/library/ykzx33wh.aspx) file:

``` xml
<asp:CompareValidatorrunat="server" CssClass="error"/>
<asp:CustomValidatorrunat="server" CssClass="error"/>
<asp:RequiredFieldValidatorrunat="server" CssClass="error"/>
<belCommon:ZipCodeValidatorrunat="server" CssClass="error"/>
<belCommon:PhoneNumberValidatorrunat="server" CssClass="error"/>
```

But what if I decide to use another validator down the road? I would have to remember to add it to the skin. Knowing that I was bound to forget, I sought out another method. After doing some digging, I found that ASP.NET generates a JavaScript variable called Page_Validators. This is an array of all the validator span elements on the current page. Now that I have access to the spans, I could write a script in the site’s [Master Page](http://msdn.microsoft.com/en-us/library/wtxbf3hh.aspx) to apply the class:

``` csharp
if (Page_Validators != null) 
{ 
    for (i = 0; i < Page_Validators.length; i++) 
    { 
        Page_Validators[i].className = "error"; 
     }
}
```

To have it run when the page is loaded, I added it as an [Sys.Application.init](http://msdn.microsoft.com/en-us/library/bb397532.aspx) handler:

``` js
Sys.Application.add_init(function(sender, args) { 
    if (Page_Validators != null) { 
        for (i = 0; i < Page_Validators.length; i++) { 
            Page_Validators[i].className = "error"; 
        } 
    } 
});
```

You could also use jQuery’s [document.ready](http://docs.jquery.com/Events/ready#fn) handler:

``` js
$(document).ready(function() { 
    if (Page_Validators != null) { 
        for (i = 0; i < Page_Validators.length; i++) { 
            Page_Validators[i].className = "error"; 
        } 
    } 
});
```

