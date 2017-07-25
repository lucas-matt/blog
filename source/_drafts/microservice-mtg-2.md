---
title: "Microservices (Mind the Gap) Part 2: Decomposition to Microservices"
subtitle: "Microservices ('Mind the Gap') Part 2"
date: 2017-07-23
tags: microservices
category: Architecture
thumbnail: /images/microservice-mtg-2/chop.jpg
cover: /images/abstract-7.jpg
---

![](/images/microservice-mtg-2/chop.jpg)
<div style="text-align: right"><sub><sup>["axe"](https://www.flickr.com/photos/nothing/5150721/) ([CC BY-NC 2.0](https://creativecommons.org/licenses/by-nc/2.0/)) by [nothing](https://www.flickr.com/people/nothing/)</sup></sub></div>

---

*This post is part of a larger series on the pitfalls, problems and (anti-)patterns commonly encountered whilst adapting and running a microservice style architecture. For further entries in this series please check out the following links*:
* *{% post_link microservice-mtg-1 Part 1: Introduction %}*
* *{% post_link microservice-mtg-2 Part 2: Decomposition to Microservices %}*

---

One of the most important problems you'll encounter whilst developing and evolving a microservice architecture is that of dividing up an existing monolith or domain into a number of well defined and decoupled entities.

So how can we divide and conquer in a sensible and well defined manner. First let's look at one of the approaches taken by Domain Driven Design - specifically that of *Bounded Contexts*.

# Bounded Context

# Things that Change Together Stay Together

As already discussed, microservices should be well-defined, nicely isolated and live within their own bounded context. The ability to separate concerns into neat little packages, that can be managed and worked on separately, is what enables all that dividing to actually conquer your monster scalability problem.

Let’s quickly review two fairly simple but fundamental concepts of good software design, cohesion and coupling. From Wikipedia:

{% blockquote Wikipedia https://en.wikipedia.org/wiki/Cohesion_(computer_science) %}
**Cohesion** refers to the degree to which the elements inside a module belong together. Thus, cohesion measures the strength of relationship between pieces of functionality within a given module. For example, in highly cohesive systems functionality is strongly related.
{% endblockquote %}

{% blockquote Wikipedia https://en.wikipedia.org/wiki/Coupling_(computer_programming) %}
**Coupling** is the degree of interdependence between software modules; a measure of how closely connected two routines or modules are; the strength of the relationships between modules.
{% endblockquote %}

# Next Time

# References
* [Microservices Architecture Principle #3: small bounded contexts over one comprehensive model](http://blog.xebia.com/microservices-architecture-principle-3-small-bounded-contexts-over-one-comprehensive-model/)
* [Bounded Context](https://martinfowler.com/bliki/BoundedContext.html)
* [DDD - The Bounded Context Explained](http://blog.sapiensworks.com/post/2012/04/17/DDD-The-Bounded-Context-Explained.aspx)
