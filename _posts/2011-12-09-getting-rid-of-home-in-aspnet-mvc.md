---
layout: post
title: Getting Rid of Home in ASP.Net MVC
category: DOTNET
permalink: getting-rid-of-home-in-aspnet-mvc
---
I'm not a big fan of having urls with the "/home" in front.  For example: /home/about, /home/contact, etc.  Here is a simple way to get your home controller routes to be a little more concise using a route constraint.

Step 1: Create the route constraint.

{% highlight csharp %}
public class RootRouteConstraint<T> : IRouteConstraint {
  public bool Match(HttpContextBase httpContext, Route route, string parameterName,
    RouteValueDictionary values, RouteDirection routeDirection) {
    var rootMethodNames = typeof (T).GetMethods().Select(x=> x.Name.ToLower());
    return rootMethodNames.Contains(values["action"].ToString().ToLower());
  }
}
{% endhighlight %}

Step 2: Add a new route mapping above your default mapping that uses the route constraint that we just created.  The generic parameter should be the controller class you plan to use as your "Root" controller.

{% highlight csharp %}
  public static void RegisterRoutes(RouteCollection routes) {
    routes.IgnoreRoute("{resource}.axd/{*pathInfo}");
    routes.MapRoute("Root", "{action}",
      new { controller = "Home", action = "Index", id = UrlParameter.Optional },
      new {isMethodInHomeController = new RootRouteConstraint<HomeController>()});
    routes.MapRoute( "Default", // Route name
      "{controller}/{action}/{id}", // URL with parameters
        new {
          controller = "Home",
          action = "Index",
          id = UrlParameter.Optional } // Parameter defaults
        );
  }
{% endhighlight %}

Now you should be able to access your home controller methods like so: /about, /contact, etc.  One gotcha here is that if you happen to have an action in your root controller with the same name as one of your controller names, then you will have some conflict.  To fix that, you could add more logic to the constraint, but I would just avoid doing that altogether.
