---
title: "Microservices (Mind the Gap) Part 2: Decomposing to Microservices"
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

*This post is part of a larger series on the challenges commonly encountered whilst adapting and running a microservice style architecture. For further entries in this series please check out the following links*:
* *{% post_link microservice-mtg-1 Part 1: Introduction %}*
* *{% post_link microservice-mtg-2 Part 2: Decomposing to Microservices %}*

---

One of the most important problems you'll encounter whilst developing and evolving a microservice architecture is that of dividing up an existing monolith or domain into a number of well defined and decoupled entities.

So how can we divide and conquer in a sensible manner. First let's look at one of the approaches taken by Domain Driven Design - specifically that of *Bounded Contexts*.

# Bounded Context

When developing any application you spend much of your time modeling the real world that it is designed to serve. The terminology that emerges out of this process generally becomes accepted across the development team as a whole, forming the unified domain model for your business. These same concepts get encoded as objects with various states and behaviors inside your application.

However, once the application reaches a certain size it becomes increasingly difficult to stretch these models to cover all aspects of the business domain. For example, an 'account' will likely mean something very different to the billing department than one geared towards managing security. This can lead to a confusion of responsibilities within the model, and whilst teams will generally be using the same terminology in reality they actually mean very different things.

A solution proposed by the [Domain Driven Design](https://www.amazon.co.uk/Domain-driven-Design-Tackling-Complexity-Software/dp/0321125215) methodology is to divide up our unified model. This approach avoids the problems described by chopping these conflicting concerns into a number of partitions - each a bounded context. This allows for coherent discussion and clear modeling to take place within the bounds of a context, adhering more closely to the single responsibility principle. It also allows us to map out the relationships between each bounded context so that interactions between them are more clearly defined.

To continue our 'account' example, we would split both 'billing' and 'security' into different bounded contexts. We are then able to reason about the concerns of the model separately for each - the billing team concerning themselves more with payments, whilst security with any rights or permissions given to an account.

![some context split example]()

*service is a business domain. bounded context == microservice*


# Things that Change Together Stay Together

As already discussed, microservices should be well-defined, nicely isolated and live within their own bounded context. The ability to separate concerns into neat little packages, that can be managed and worked on separately, is what enables all that dividing to actually conquer your monster scalability problem.

Let’s quickly review two fairly simple but fundamental concepts of good software design, cohesion and coupling. From Wikipedia:

{% blockquote Wikipedia https://en.wikipedia.org/wiki/Cohesion_(computer_science) %}
**Cohesion** refers to the degree to which the elements inside a module belong together. Thus, cohesion measures the strength of relationship between pieces of functionality within a given module. For example, in highly cohesive systems functionality is strongly related.
{% endblockquote %}

{% blockquote Wikipedia https://en.wikipedia.org/wiki/Coupling_(computer_programming) %}
**Coupling** is the degree of interdependence between software modules; a measure of how closely connected two routines or modules are; the strength of the relationships between modules.
{% endblockquote %}

*if changing components together then should be merged*
*drive modularity through things that change at the same time*

# Strangler Pattern

*strangler pattern*

# Next Time



# References
* [Microservices Architecture Principle #3: small bounded contexts over one comprehensive model](http://blog.xebia.com/microservices-architecture-principle-3-small-bounded-contexts-over-one-comprehensive-model/)
* [Bounded Context](https://martinfowler.com/bliki/BoundedContext.html)
* [DDD - The Bounded Context Explained](http://blog.sapiensworks.com/post/2012/04/17/DDD-The-Bounded-Context-Explained.aspx)
* [Domain Driven Design](https://www.amazon.co.uk/Domain-driven-Design-Tackling-Complexity-Software/dp/0321125215)
