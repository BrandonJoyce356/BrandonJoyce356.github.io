---
layout: post
title: My Top 3 Reasons Why Dependency Injection is Not for Ruby 
permalink: top-3-reasons-dependency-injection-not-for-ruby 
---

A little over 6 months ago I took a full time Ruby development job at CoverMyMeds.  
I'm primary a Microsoft stack developer, but I've dabbled with Ruby (and other languages) for side projects since around 2008. 
Going full-time Ruby made me want to work much harder on making sure my Ruby code was idiomatic. 
I've learned some really cool things, but the thing that was somewhat challenging was giving up dependency injection.

What is Dependency Injection?
=============================
A basic object-oriented pattern for complying with Dependency Inversion Principle (DIP).
A C# example would be to pass dependencies to your constructor rather than using the 'new' keyword.
{% highlight csharp %}
  public class AdminController {
    
    IUserService _userService;

    public AdminController(IUserService userService) {
      _userService = userService;
    }

    public ActionResult Index() {
      var currentUser = _userService.CurrentUser();
      return View();
    }
  }
{% endhighlight %}
