---
layout: post
title: JQuery, AJAX, and ASMX
permalink: jquery-ajax-and-asmx
---

JQuery is just awesome! One of the things that I recently did that I got 
a kick out of was doing an AJAX call to an ASMX webservice. 
It's pretty straightforward once you know how to do it... 

Step 1: Un-Comment this line of code from your webservice...

{% highlight xml %}
[System.Web.Script.Services.ScriptService]
{% endhighlight %}

Step 2: Put the AJAX JQuery Function in a script tag... 

{% highlight javascript %}
$.ajax({ 
  type: "POST", 
  url: "/WebserviceName.asmx/MethodName", 
  data: "{}", //A stringified json object
  contentType: "application/json; charset=utf-8", 
  dataType: "json", 
  success: function(json) { console.log(json.message); } 
});
{% endhighlight %}

That's pretty much it! If your method doesn't except parameters then you should just use "{ }" for the data part. Also, you would probably want to attach this function to a button click event or something else on your web page instead of just having it fire when the page is loaded. 

This method of doing an AJAX call is going to perform better than with update panel because you are strictly transferring JSON data. Update panel is still doing a full postback behind the scenes.
