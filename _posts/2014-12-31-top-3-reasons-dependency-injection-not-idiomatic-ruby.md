---
layout: post
title: Top 3 Reasons Why Dependency Injection Containers Are Not Idiomatic Ruby
category: Ruby
permalink: top-3-reasons-dependency-injection-containers-are-not-idiomatic-ruby
---

If you aren't already aware of what dependency injection (and containers) are then you should begin by reading my post [What Is Dependency Injection?]({% post_url 2014-12-30-what-is-dependency-injection %})

A little over 6 months ago I took a full-time Ruby development job at [CoverMyMeds](https://www.covermymeds.com)
My past has primarily been as a Microsoft stack developer, but I've dabbled with Ruby (and other languages) for side projects since around 2008.
Going full-time with Ruby made me want to work much harder on making sure my Ruby code was idiomatic.
I've learned some really cool things, but one thing that was somewhat challenging was giving up dependency injection.
This post will cover the top 3 reasons I think dependency injection has diminished value in Ruby.

1: Isolated Testing Doesn't Require Abstract Types In Ruby
=======================================================
In a static language like C#, you'll typically want to depend on an interface so you can use stubbed or mocked implementations during testing.
In a dynamic language like Ruby, the interface isn't needed.
Also, you can stub methods on objects even without injecting your own implementation.
Here's an example test using RSpec to setup expectations.

{% highlight ruby %}
  describe "MyObject" do

    subject { MyObject.new }

    let(:test_data) { [ 'testing'] }

    it "gets data from a dependency" do
      #stubbing the Dependency.get_data method to return whatever we want
      expect(Dependency).to receive(:get_data).and_return(test_data)
      subject.process
    end

  end
{% endhighlight %}

2: Simplified Initializers
==========================
One of the advantages of using a container for dependency injection in C# is that you can avoid using the 'new' keyword.
This means that the constructor signature can change without going through your code and changing every place that this class is being instantiated.
In Ruby however, if you're avoiding the dependency injection pattern altogether, then you are much less likely to suffer from this problem.
Adding a dependency to a Ruby class doesn't typically change the initializer.

{% highlight ruby %}
  class Customer
    # these parameters stick to domain related things
    def initialize name
      @name = name
    end

    def save
      Database.save self
      #We can add a dependency on a logging class without injecting it
      Logger.log "Saving customer: #{self.name}"
    end
  end

{% endhighlight %}

This might seem super-obvious, but if you're used to C# like me, you know what it's like to be bitten by code like this when trying to isolate classes for testing.
Remember though, that's not a problem anymore because of #1!

Another nifty thing you might utilize would be hash initializers.  This language feature is useful in some situations when you have a

3: Dependency Injection Without a Container
================================================
You don't need a container to do dependency injection!
If you have a case where you really want to pass dependencies in to the initializer, just do it.

{% highlight ruby %}
  #instead of this
  class MyObject
    def process
      Dependency.new.get_data
    end
  end

  #do this!
  class MyObject
    def initialize dependency
      @dependency = dependency
    end

    def process
      @dependency.get_data
    end
  end

{% endhighlight %}

The great thing is that you're not preemptively doing this just for testing purposes.
You can use this when it fits into your design, and avoid it otherwise.
And no need to define an interface. Yay dynamic typing!

If you want to hear me babble about this topic in a lightning talk at Steel City Ruby Conf, check out the video! This was a spontaneous talk so I didn't cover everything I wanted, but I did my best!

<iframe width="560" height="315" src="//www.youtube.com/embed/nHl5Fx5KK6U" frameborder="0" allowfullscreen></iframe>

As always, I'm curious for feedback.
