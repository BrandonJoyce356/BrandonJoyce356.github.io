---
layout: post
title: SQL Footprint - A gem for working with legacy Rails apps
category: Ruby
permalink: sql-footprint-gem
---

I just wanted to leave some notes on [a new gem](https://github.com/covermymeds/sql_footprint) that I and some folks from my day job put together to solve a couple of specific problems.
The sql_footprint gem tracks queries that ActiveRecord uses when you run your tests, and creates a footprint.sql file showing what queries are actually being executed.
It also canonicalizes and dedupes similar queries so you're just seeing unique types of queries in the footprint.

## Why is this useful?
- Helps to identify potentially dangerous/bottleneck queries more clearly within pull requests/code review.
- Helps to identify db dependencies when breaking a monolith architecture down into separate services and databases.

## Identifying bottlenecks
SQL Footprint shows you when you're doing something in the DB that hasn't already been done.
Imagine finding this code in a pull request:

{% highlight ruby %}
Customer.where(last_name: "Smith")
{% endhighlight %}

Seems harmless, but do we already query by last name in our app?  Rather than search your entire code base looking for queries that might be doing something similar, you can simply look at your SQL Footprint:

{% highlight sql %}
Select * from customers where last_name = 'value-redacted';
{% endhighlight %}

If you see this in the diff for your sql footprint, you know you're breaking new ground with this query.
You should probably talk to your database folks about any optimization concerns.
Likewise if you see the removal of expensive queries, maybe you can go ask your DBAs to buy you a beer?  SQL Footprint buys you beer.  That's just how it works. :)

## Breaking down monoliths
One of the things that can be difficult in breaking a monolith architecture into smaller services is deciding if/when/how to break down the monolith sql database that you likely have all your data in.
SQL Footprint is just one way of getting a good view of what objects in your database are depended upon by application components.
Especially if you're taking the approach of breaking things into APIs/Microservices before splitting the DB.
By getting a service-by-service footprint, you should be able to see where database dependencies overlap and where they don't.
For example, if your "CustomerService" is the only service that ever touches the customers table, you know you can (if you want) pull that table into it's own database and let the custoemrs service "own" that.

Hope this is useful for some of you!  I know we're making great use of it so far.  Let me know if you have any questions or suggestions for improvement.

