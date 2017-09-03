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

A ton of the functionality provided here is backed by the Netflix OSS stack. Service discovery, load balancing, fault tolerance and gateway routing features are all supporting by Netflix's toolset, although the full stack does much more than this, of course. In the picture below, I've marked out the specific libraries we'll be checking out later, with a few notes as to their general purpose.

![](/images/spring-microservices/netflix.png)

---

# Configuration Management with Spring Cloud

At the tail end of the last century, NASA sent an orbiter to Mars with the intention of surveying the red planet to understand its water history and to search for the traces of evidence to suggest that life had once existed there. The spacecraft arrived, after a grueling ten month journey, only to have disaster strike and it burn up in the Martian atmosphere after flying almost 105 miles closer to the planet's surface then intended. The reason for this, it turned out afterwards, was due to a misunderstanding between two separate development teams as to the units of force used throughout the system. On one hand, propulsion engineers at Lockheed Martin had used their standard expression of force in pounds. However, in space engineering, the commonly used units are newtons, and NASA engineers hadn't thought to question any mismatch when integrating components. One pound of force is around 4.45 newtons, providing enough of a difference to cause the disaster that occurred.

So how does this relate at all to configuration management? Well, it's a fairly crude example of the importance of a single source of truth, and how miscommunication across components in a system can result in a catastrophic outcome. The principles at work here apply to the services within a distributed system - most specifically, in this case, related to configuration of those services, and the concept of *configuration drift*. Let's look at a definition of this:

{% blockquote DZone https://dzone.com/articles/configuration-drift %}
Configuration Drift is the phenomenon where running servers in an infrastructure become more and more different as time goes on, due to manual ad-hoc changes and updates, and general entropy.
{% endblockquote %}

Instances of microservices should be totally unremarkable, and completely replaceable. Having any chance of a unique configuration being introduced to one service over others could cause unexpected issues within a production setting - and any kind of properties/configuration file tied to a single instance of a service provides that chance.

![](/images/spring-microservices/kaboom.png)

This is where a centralized configuration strategy can help. All services point at that single source of truth, making divergence of configuration across them much less likely. As a second bonus feature the ability to change that one piece of information to affect all dependents instantly, can streamline general management of configuration within your distributed application.

![](/images/spring-microservices/config.png)

# Service Discovery with Ribbon & Eureka!

Another important aspect of a distributed system is how you actually connect all those moving parts together in the first place! Of course, it's easy to statically configure a set of addresses on service boot, but what if one of those endpoints disappears or becomes unhealthy?

Eureka! and it's partner, Ribbon, were designed to help solve this problem. As a service starts it registers itself with the central Eureka service. This allows any dependent service to find out who to talk to via this central point.

Eureka keeps tabs on a service instance by prodding it's health-check API to ensure that it is available and happy to serve. If an instance is found to be unavailable or reporting issues, it is removed from the working list.

Ribbon keeps the client side of this arrangement simple. It is a request side library that keeps in touch with Eureka to keep track of those addresses that serve a certain function. It abstracts away the physical addresses for a location transparent reference which we can use within our code to decouple our service from any of those upstream.

![](/images/spring-microservices/eureka.png)

# Failing Successfully with Hystrix

Netflix's Hystrix is a fault tolerance library designed to prevent cascading failures across a distributed system - where failure is almost certainly going to occur at some point.

## Circuit Breaker

![](/images/spring-microservices/circuitbreaker.png)

## Bulkhead

![](/images/spring-microservices/bulkhead.png)

# Zuul

![](/images/spring-microservices/zuul.png)

# Zipkin

![](/images/spring-microservices/zipkin.png)

# Event Based
![](/images/spring-microservices/messages.png)
