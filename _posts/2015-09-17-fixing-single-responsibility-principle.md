---
layout: post
title: Fixing Single Responsibility Principle
permalink: fixing-single-responsibility-principle
---

Single Responsibility Principle is broken!

Don't get me wrong.
I love SRP.
I believe it was Corey Haines that told me once that SRP is likely the basis for all other principles and patterns.
That may or may not be true, but the fact is, this principle is super important and well known among software developers.

#### Single Responsibility Principle (According to Uncle Bob)
> A class should have one, and only one, reason to change.

I know this isn't the only definition of SRP, but it's a common one that a lot of people are first introduced to.

How could it be that something so fundamental to the way we write code could be flawed?
I think the answer lies in the many discussions I've had about SRP:
"If you exactly follow SRP, your code will be terrible." or "You can take it too far."
I don't hear that nearly as much about other principles.
Why is that?

I think the reason for this lies in the word "single" and the phrasing of "only one reason".
A design that truly attempts to break down classes to the level that you could only conceive of one reason to change that class is not always the most communicative design.
Sometimes, it's best to keep a couple of closely related "responsibilities" together in a class just for clarity.
This is especially true when those two responsibilities are not very complex.
Also, you can usually move these things to separate classes later as the design grows and changes.
I think this is typically what people mean when they are looking for that "right" level of SRP in their design.

Having said all that, I am merely suggesting that we slightly refine this principle to more accurately represent this pragmatic process for applying SRP in our design.
In keeping with the "S" in solid, I think this would be a more appropriate principal:

#### Suitable Responsibility Principle
> Every class should have a minimum and cohesive set of reasons to change.

This principle is less specific about an ideal number of reasons for change, but highlights the importance that these reasons must be cohesive.
We're leaving room for pragmatism while still clearly defining what makes objects too complex.

Maybe I've got the wrong idea here.
Maybe principles are supposed to be more mathematic and exact.
I just think "Suitable Responsibility Principle" more effectively encourages an appropriate design for the situation.

I'm very curious to see what folks think of these crazy thoughts!  Please comment!
