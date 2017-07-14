---
title: "Microservices: Pitfalls, Problems & (Anti-)Patterns"
date: 2017-07-14
tags: microservices
cover: /images/abstract-6.jpg
---
![Mind the Gap](mindthegap.jpg)


# Introduction (aka 'The Positive Bit')

The microservices architectural style seems to be continuing with its ever-rising popularity, and with good reason. There is a lot to be gained from adopting this model to ensure that your large application can be developed at a fast pace, scaled appropriately, and delivered to production with higher frequency and less overall risk. Let's discuss some of the key benefits of a microservice architecture before we begin to tread carefully around some of its more scary attributes.

### Scaling Your Development Team

Monolithic applications aren't actually a bad thing (despite the constant bad press), but once your product reaches a certain size you will likely hit several scalability challenges. The first of these will likely come as an organizational challenge rather than anything grounded in technology.

Many of us have worked on one of those humongous Java applications which contain hundreds of thousands of lines of code, get deployed into an enterprise application server (such as JBoss or WebSphere) and are supported by some large RDBMS. To keep up with the competition many teams will need develop features on this monolith in parallel, and here's where the problems creep in. Merge car crashes, leaky modules and an unwieldy test framework soon make the application a nightmare platform on which to develop.

Microservices to the rescue! When we no longer have one gigantic project, but instead many small cleanly partitioned pieces, then work on various features is also partitioned. This gives us cleaner development in an isolated and much more mangeable development environment.

![Monolithic boulder broken down]()

A common, but excellent way, to picture this difference is to imagine a number of workers chipping away at a large boulder. In the monolithic case they struggle to gather around the surface well enough to get their work done without bashing away at the hands of the person next to them. When broken down into smaller pieces the surface area is greatly increased, and the workers can happily chip away in much greater comfort without bloody hands and broken thumbs.

### Scaling Your Application

When your killer app does eventually go viral and needs to be scaled horizontally, with a monolith you have no option but to ramp up 'everything and the kitchen sink' regardless of whether it's a bottleneck or not. This can clearly result in a great deal of wasted resource to support the redundant parts of the deployment that have hitched along for the ride.

In the case of microservices, because they're already nicely chopped up into sensible parts, then we can scale out only the bits that we need allowing us to much more finely tune our platform to the traffic hitting it.

### Reducing Deployment Risk

In a similar vein to the above, the divide and conquer approach of microservices can also, *if done correctly*, make your deployment simpler and more stable.

In the case of a monolith any upgrade will, by it's nature, include all changes made to the monolith since the last release. If something were to go wrong during the upgrade then the cause could come from any number of new features, not to mention the sheer complexity you hit in just getting a large application up and running in the first place.

By splitting out the moving parts we can also separate out their deployment. Smaller, more controlled, deployments result in less surprise, less uncertainty and makes quickly reverting back to a known state a more welcoming prospect than rolling back a huge upgrade.

### Now for the Spooky Part

![Spooky Part]()

But I'm not here to gush about the wonders of a microservice architecture - and admittedly there are many potential benefits available to you. Rather I'm here to discuss the scary bits; the parts that can keep you up at night in a cold sweat and make your work-day feel like herding wildcats around a data centre - and of course some common approaches on how to avoid getting to that point in the first place.




# References
* [Production Ready Microservices](http://shop.oreilly.com/product/0636920053675.do)
* [Reactive Microservices Architecture](http://www.oreilly.com/programming/free/reactive-microservices-architecture-orm.csp)
* [Seven Deadly Sins of Microservices](https://www.infoq.com/presentations/7-sins-microservices)
* [Martin Fowler: Microservice Prerequisites](https://martinfowler.com/bliki/MicroservicePrerequisites.html)
* [Martin Fowler: Microservices](https://martinfowler.com/articles/microservices.html)
