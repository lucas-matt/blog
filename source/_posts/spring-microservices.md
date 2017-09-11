---
title: Spring Cloud in 10 Bad Cartoons
tags:
  - spring
  - java
  - microservices
thumbnail: /images/spring-microservices/spring.jpg
category: Programming
date: 2017-08-22 00:00:00
---


![](/images/spring-microservices/spring.jpg)

*A quick tour of the combined Spring Cloud / Netflix OSS microservice stack through some pretty terrible drawings, inspired by John Carnell's book [Spring Microservices in Action](https://www.manning.com/books/spring-microservices-in-action) (the subject, that is, not the awful pictures)*

---

Building well designed applications using microservices can require a great deal of maturity. Aspects such as service discovery, load balancing and gracefully handling failure are effectively mandatory, but can be painful to implement well.

[Spring Cloud](http://projects.spring.io/spring-cloud/) pulls together a number of well worn tools that help make a number of the core patterns of distributed systems simpler to wire up and manage. More specifically this can involve technology such as Consul, Zookeeper and the Netflix OSS stack.

We'll now check out some of the patterns made available to you by utilizing Spring Cloud, and related tooling, through the medium of terrible drawings.

# Netflix OSS

A ton of the functionality provided here is backed by the Netflix OSS stack. Service discovery, load balancing, fault tolerance and gateway routing features are all supporting by Netflix's toolset, although the full stack does much more than this. In the picture below, I've marked out the specific libraries we'll be checking out later, with a few notes as to their general purpose.

![](/images/spring-microservices/netflix.png)

---

# Configuration Management with Spring Cloud

At the tail end of the last century, NASA sent an orbiter to Mars with the intention of surveying the red planet to understand its water history and to search for the traces of evidence to suggest that life had once existed there. The spacecraft arrived, after a grueling ten month journey, only for disaster to strike and it burn up in the Martian atmosphere after flying almost 105 miles closer to the planet's surface then intended. The reason for this, it turned out afterwards, was due to a misunderstanding between two separate development teams as to the units of force used throughout the system. On one hand, propulsion engineers at Lockheed Martin had used their standard expression of force in pounds. However, in space engineering, the commonly used units are newtons, and NASA engineers hadn't thought to question any mismatch when integrating components. One pound of force is around 4.45 newtons, providing enough of a difference to cause the disaster.

So how does this relate at all to configuration management? Well, it's a fairly crude example of the importance of a single source of truth, and how miscommunication across components in a system can result in a catastrophic outcome. The principles at work here apply to the services within a distributed system - most specifically, in this case, related to configuration of those services, and the concept of *configuration drift*. Let's look at a definition of this:

{% blockquote DZone https://dzone.com/articles/configuration-drift %}
Configuration Drift is the phenomenon where running servers in an infrastructure become more and more different as time goes on, due to manual ad-hoc changes and updates, and general entropy.
{% endblockquote %}

Instances of microservices should be totally unremarkable, and completely replaceable. Having any chance of a unique configuration being introduced to one service over others could cause unexpected issues within a production setting - and any kind of property/configuration file tied to a single instance of a service provides that chance.

![](/images/spring-microservices/kaboom.png)

This is where a centralized configuration strategy can help. All services point at that single source of truth, making divergence of configuration across them much less likely. As a second bonus feature the ability to change that one piece of information to affect all dependents instantly, can streamline general management of configuration within your distributed application.

![](/images/spring-microservices/config.png)

# Service Discovery with Ribbon & Eureka!

Another important aspect of a distributed system is how you actually connect all those moving parts together in the first place! Of course, it's easy to statically configure a set of addresses on service boot, but what if one of those endpoints disappears or becomes unhealthy?

Eureka! and it's partner, Ribbon, were designed to help solve this problem. As a service starts it registers itself with the central Eureka service. This allows any dependent service to find out who to talk to via this central point.

Eureka keeps tabs on a service instance by prodding it's health-check API to ensure that it is available and happy to serve. If an instance is found to be unavailable or reporting issues, it is removed from the working list.

Ribbon keeps the client side of this arrangement simple. It is a request side library that keeps in touch with Eureka to keep track of those addresses that serve a certain function. It abstracts away the physical addresses for a location-transparent reference which we can use within our code to decouple our service from any of those upstream.

![](/images/spring-microservices/eureka.png)

# Failing Successfully with Hystrix

Netflix's Hystrix is a fault tolerance library designed to prevent cascading failures across a distributed system - a place where failure is almost certainly going to occur at some point.

Application architecture is generally well enough designed to cater for large-scale failure, and by this I mean situations like a full server outage. Databases are replicated so that they can lose a cluster member and still remain unhindered. API calls are often load balanced across a number of identical instances of an application to avoid any single point of failure.

However, smaller scale failure, or a downward spiral of QoS, are generally less well handled. Specifically aspects such as intermittent failure and ever-increasing latency of upstream responses are not well catered for and so requests eventually back up, and overwhelm the system.

## Circuit Breakers

A circuit breaker functions in a manner similar to its electrical counterpart. But rather than detecting an electrical surge it tries to prevent a situation where a struggling upstream service is becoming increasingly more stressed due to an overwhelming number of requests. It does this by monitoring the lifecycle of a remote service call. If the latency begins to creep up the connection is cut protecting its dependency.

Once disconnected, the behaviour of the circuit breaker changes somewhat. As calls continue to enter the service to which the circuit breaker belongs, the upstream endpoint is tested until we see that good service is resumed, at which point the circuit breaker is closed again and requests are allowed to flow freely once again.

![](/images/spring-microservices/circuitbreaker.png)

As you can imagine, if the circuit breaker is open the requests are unable to successfully complete. Although beneficial to the upstream service being protected, it's not great for the client that made the request in the first place.

This is where an extension of this pattern becomes useful.

### Fallbacks

Instead of just allowing the request to crash 'n' burn, we can fail more gracefully by providing a fallback to our incomplete API call. This can be provided by a cache, an alternative or even just plain old stubbed data. The important thing is that to an outsider it looks just like the real thing.

For example - let's say you are a service providing some personalized recommendations, but the recommendation engine has been circuit broken. By providing some general pre-cached recommendations an end user wouldn't notice the difference, unless they *really* started poking around.

## Bulkhead

Have you ever experienced a performance issue where a slow running resource, be it a database or API call, has caused requests to back up and eventually consume all the threads in your app. The reason for this is that your app is acting like a big hollow rowing boat - one leak and water eventually consumes the whole thing.

The bulkhead pattern (in reference to a ship's bulk heads) is a way to isolate different remote calls into their own thread pools. If one remote resource causes requests to queue up then the problem is isolated to that single resource, protecting the rest of the application to carry on as normal as possible.

![](/images/spring-microservices/bulkhead.png)

# Zuul

Zuul (as in that nightmarish dog-monster thing from Ghostbusters) acts as a gatekeeper to your full suite of microservices. This single entrypoint for all requests allows you to manage several cross-cutting concerns in one place. Aspects such as security, monitoring and logging, to name but a few.

Zuul can intercept a request at potentially three separate points in the lifecycle of a request, allowing you to decorate with additional functionality as appropriate.

* **pre filters** add custom logic to process the request as it enters your "domain"
* **post filters** are the final stop as a response leaves your platform. For example, to log the completion of the request.
* **route filters** intercept the request before it travels upstream and gives you the chance to alter its destination. Great for managing A/B testing, and similar strategies.

In addition to this Zuul integrates seamlessly with the Eureka service discovery engine, to be able to dynamically determine healthy upstream resources.

![](/images/spring-microservices/zuul.png)

# Event Based Architecture

Often microservices communicate via RESTful API calls. REST implies synchronicity because the request/response are naturally tied together and as such cause a fairly tight coupling between two services.

Unfortunately, this tight coupling causes some complexity in managing communication in aspects such as fault tolerance (i.e the Hystrix library we discussed earlier). Synchronous communication is also much more affected by general slowness making graceful degradation a tricky prospect.

By decoupling services through some kind of message bus we gain many advantages through asynchronous messaging. The ability to easily scale, to cope with outages and downtime, and to evolve your architecture to support additional consumers, are all great benefits that can make your system much more resilient and flexible.

![](/images/spring-microservices/messages.png)

# Zipkin

With all of this technology chatting away in a distributed fashion it can make debugging a production issue quite a challenge, to say the least.

The [OpenTracing](http://opentracing.io/) initiative aims to alleviate this problem by providing a vendor, language and framework independent solution - of which [Zipkin](http://zipkin.io/) is a member.

A single request flow, or trace, is started at our gateway (e.g. Zuul) and propagates through the full traversal. Each trace is broken down into a number of spans which represent some service processing step such as a database call.

Traces are captured and logged to a central service for only a small sample of requests (by default 10%). The final outcome is a visual representation of the lifecycle of a request through your system, accompanied by some key metrics at each stage (span) allowing you to track down those sneaky areas of concern. 

![](/images/spring-microservices/zipkin.png)

# Summary

Spring Cloud can greatly simplify the development of a suite of robust, cleanly integrated microservices. However, there is an open question as to how well this integration stretches to non-spring services - and, of course, part of the microservice mantra is to use the right tool for each job, which may lead to a more diverse technology footprint across many teams. I suppose this may be the case in which a service mesh makes most sense. But if you are starting out with a handful of Java based services, you could do much worse than adopting the Spring Cloud framework and it's associates.
