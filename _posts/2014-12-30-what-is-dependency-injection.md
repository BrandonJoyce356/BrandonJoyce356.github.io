---
layout: post
title: What is Dependency Injection? 
permalink: what-is-dependency-injection 
---

Dependency injection is a basic object-oriented pattern for complying with [Dependency Inversion Principle](http://en.wikipedia.org/wiki/Dependency_inversion_principle) (DIP).
Note they are not the same thing.  Injection is just a sort of implementation of DIP.
A C# example would be to pass dependencies to your constructor rather than using the 'new' keyword.

*Non-Injected Example*
{% highlight csharp %}
  public class WithoutInjectionController : Controller {
    public ActionResult Index() {
      //Notice here we are "reaching" directly for a concrete type here
      var userService = new UserService();
      var currentUser = userService.CurrentUser();
      return View();
    }
  }
{% endhighlight %}

*Injected Example*
{% highlight csharp %}
  public class InjectedController : Controller {
    IUserService _userService;

    //With injection, we are asking for an implementation to be given to us.
    public AdminController(IUserService userService) {
      _userService = userService;
    }

    public ActionResult Index() {
      var currentUser = _userService.CurrentUser();
      return View();
    }
  }
{% endhighlight %}

It's important to differentiate this simple pattern from a full-fledged "Dependency Injection Container".

What are Dependency Injection Containers?
=========================================
Dependency injection containers are tools that provide helpful features over a code base that implements the dependency injection pattern described above.
These containers are primarily used to define your dependencies and automatically resolve instances of dependencies when instantiating an object.
Another feature often used is the ability to control the lifetime scoping of your instances.  Think singleton, transient, and unit of work.
Popular containers you've probably heard of from the C# arena would be Ninject, Autofac, StructureMap, etc.
