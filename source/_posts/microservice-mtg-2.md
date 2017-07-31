---
title: 'Microservices Part 2: Breaking Up That Monolith'
tags: microservices
category: Architecture
thumbnail: /images/microservice-mtg-2/chop.jpg
cover: /images/abstract-7.jpg
date: 2017-07-31 00:00:00
---


![](/images/microservice-mtg-2/chop.jpg)
<div style="text-align: right"><sub><sup>["axe"](https://www.flickr.com/photos/nothing/5150721/) ([CC BY-NC 2.0](https://creativecommons.org/licenses/by-nc/2.0/)) by [nothing](https://www.flickr.com/people/nothing/)</sup></sub></div>

---

*This post is part of a larger series on the challenges commonly encountered whilst adapting and running a microservice style architecture. For further entries in this series please check out the following links*:
* *{% post_link microservice-mtg-1 Part 1: Introduction %}*
* *{% post_link microservice-mtg-2 Part 2: Breaking Up That Monolith %}*
* Part 3: Communication & Evolution
* Part 4: Organization & Standardization
* Part 5: Prepare to Fail

---

One of the most important problems you'll encounter whilst developing and evolving a microservice architecture is that of dividing up an existing monolith or domain into a number of well defined and decoupled entities.

So how can we divide and conquer in a sensible manner? First let's look at one of the approaches taken by Domain Driven Design - specifically that of *Bounded Contexts*.

# Bounded Context

When developing any application you spend much of your time modeling the real world that it is designed to serve. The terminology that emerges out of this process generally becomes accepted across the development team as a whole, forming the unified domain model for your business. These same concepts get encoded as objects with various states and behaviors inside your application.

However, once the application reaches a certain size it becomes increasingly difficult to stretch these models to cover all aspects of the business domain. For example, an 'account' will likely mean something very different to the billing department than one geared towards managing security. This can lead to a confusion of responsibilities within the model, and whilst teams will generally develop using the same terminology in reality they actually mean very different things.

A solution proposed by the [Domain Driven Design](https://www.amazon.co.uk/Domain-driven-Design-Tackling-Complexity-Software/dp/0321125215) methodology is to divide up our unified model. This approach improves upon the above by chopping these conflicting concerns into a number of separate areas - each a bounded context. This allows for coherent discussion and clear modeling to take place within the bounds of a context, adhering more closely to the single responsibility principle. It also allows us to map out the relationships between each bounded context so that the interactions between them are more clearly defined.

To continue our 'account' example, we would split both 'billing' and 'security' into different bounded contexts. We are then able to reason about the concerns of the model separately - the billing team concerning themselves more with payments, whilst security with any rights or permissions given to an account.

![Bounded Context Example](/images/microservice-mtg-2/bounded-context.png)

It follows from this, that the way we breakup our application into services corresponds very naturally to the bounded contexts we define. Taking this approach allows us to reduce the amount of knowledge that any one team has to keep in mind, as the focus only has to be within a specific context. This also leads to more cleanly separated entities, which should reduce external dependencies and simplify over-complicated chatter.

In short, a service should correspond to a single business domain, and not cross boundaries.


# Things that Change Together Stay Together

The ability to separate business concerns into neat little packages that can be managed and worked on separately is what enables all that *dividing* to actually *conquer* your monster scalability problem. Let’s quickly review two fairly simple but fundamental concepts of good software design:

{% blockquote Wikipedia https://en.wikipedia.org/wiki/Cohesion_(computer_science) %}
**Cohesion** refers to the degree to which the elements inside a module belong together. Thus, cohesion measures the strength of relationship between pieces of functionality within a given module. For example, in highly cohesive systems functionality is strongly related.
{% endblockquote %}

{% blockquote Wikipedia https://en.wikipedia.org/wiki/Coupling_(computer_programming) %}
**Coupling** is the degree of interdependence between software modules; a measure of how closely connected two routines or modules are; the strength of the relationships between modules.
{% endblockquote %}

After revisiting these concepts it becomes clearer that microservices should be highly cohesive but loosely coupled. Concepts and functionality that are strongly related need to be kept within the bounds of the same service. Conversely, more weakly related concepts should tend to exist within separate modules.

An architecture that keeps correlated concerns together and pushes unrelated concepts apart allows for more robust development and deployment strategies. Due to this loose coupling of components, the impacts of one service on another should be simplified to allow for both parallel development (with fewer blocking elements between teams) and an independent deployment lifecycle - i.e. avoiding the need to orchestrate delivery between whole suites of services.

If you find that the addition of a feature to your application requires a tough co-ordination effort, then you should consider whether your microservices really are cohesive and loosely coupled, or if abstractions and business logic are crossing several boundaries. If several services are always changing in step with one another then the question really is whether they're partitioned well-enough? If the answer seems to be no, then it's time to start merging and refactoring those components to achieve a smarter separation.

# Strangler Pattern

![](/images/microservice-mtg-2/strangler-real.jpg)
<div style="text-align: right"><sub><sup>["IMG_9322/Ile Maurice/Trou Aux Biches/Big"](https://www.flickr.com/photos/dany13/30109648515/) ([CC BY 2.0](https://creativecommons.org/licenses/by/2.0/)) by [dany13](https://www.flickr.com/people/dany13/)</sup></sub></div>

It's very rare that a greenfield project lands in your lap. Unfortunately for us developers most of our days are spent maintaining and evolving existing systems. This is definitely something that microservices (done well) can help you tackle more easily in future, but let's come back to today's reality. How we can sensibly refactor an existing ball-of-mud application into a more managable architecture?

## Beware the Big Rewrite

After decades of high profile failures it has generally become a natural intuition of software professionals to avoid "The Big Rewrite", but it's worth a cautionary mention anyhow. 

*Second System Syndrome* refers to the common outcome of replacing a profitable, but flawed, system with a complete rewrite that generally misses much of the point of what made the original system successful in the first place.

{% blockquote Mythical Man Month https://www.goodreads.com/book/show/13629.The_Mythical_Man_Month %}
When it seems to be working well, designers turn their attention to a more elaborate second system, which is often bloated and grandiose and fails due to its over-ambitious design. In the meantime, the first system may also fail because it was abandoned and not continually refined.
{% endblockquote %}

## What is the 'Strangler Pattern'?

The Strangler Pattern is a method of slowly wrapping and replacing an existing system, usually a monolith, in a slow and methodical fashion. It is named after the strangler fig vines found in tropical climates. These vines slowly grow upon an existing tree, eventually covering (and effectively replacing) the host.

This same pattern can be applied in evolving a piece of software. One by one, each part of the application (potentially identified by a bounded context) is refactored into a new service and spun out on its own. A façade provides the single entry point to your API disguising those parts of the app that have been migrated vs those which are still waiting in line for attention. 

{% img /images/microservice-mtg-2/strangler.jpg 400 "Stranger Pattern" %}

This iterative process gives us many benefits, including:
* Keeping each refactoring managable due to its small, well defined, context
* Constant validation of the new functionality vs the old in a real-world, production, scenario
* Ability to handle failure more gracefully due to a rollback being as simple as redirecting the façade's requests

# Next Time

Next time, we'll look into the communication patterns available to connect your microservices together in a maintainable and robust way. 

# References
* [Microservices Architecture Principle #3: small bounded contexts over one comprehensive model](http://blog.xebia.com/microservices-architecture-principle-3-small-bounded-contexts-over-one-comprehensive-model/)
* [Bounded Context](https://martinfowler.com/bliki/BoundedContext.html)
* [DDD - The Bounded Context Explained](http://blog.sapiensworks.com/post/2012/04/17/DDD-The-Bounded-Context-Explained.aspx)
* [Domain Driven Design](https://www.amazon.co.uk/Domain-driven-Design-Tackling-Complexity-Software/dp/0321125215)
* [Martin Fowler - Strangler Application](https://www.martinfowler.com/bliki/StranglerApplication.html)
* [Apply the Strangler Application pattern to microservices applications](https://www.ibm.com/developerworks/cloud/library/cl-strangler-application-pattern-microservices-apps-trs/index.html)
* [Mythical Man Month](https://www.amazon.co.uk/Mythical-Man-month-Essays-Software-Engineering/dp/0201835959/ref=sr_1_3?s=books&ie=UTF8&qid=1501257288&sr=1-3&keywords=mythical+man+month)
* [Strangler Pattern](https://docs.microsoft.com/en-us/azure/architecture/patterns/strangler)
