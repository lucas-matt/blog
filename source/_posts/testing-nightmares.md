---
title: 6(66) Nightmares of Microservice Testing
tags:
    - microservices
    - testing
date: 2018-03-13 19:35:18
category: Testing
thumbnail: /images/nightmare.jpg
---


![](/images/nightmare.jpg)
<div style="text-align: right"><sub><sup>["The Stuff of nightmares"](https://www.flickr.com/photos/ollierb/14572262390/) ([CC BY-NC-ND 2.0](https://creativecommons.org/licenses/by-nc-nd/2.0/)) by [ollierb](https://www.flickr.com/people/ollierb/)</sup></sub></div>

Tests are clearly an extremely important port of a developer's day-to-day. They provide us with a safety net as we hack away at the rotten roots of our codebase, allowing us to feel more like a real engineer and less like we're walking through an antiques shop wearing a 12th century suit of armour.

But as Dante said - there's a special place in hell for evil-doing programmers, where the tests run ever so slowly, constantly flap and the coffee never ever brews! - or at least I'm pretty sure that's what Inferno describes having never actually read it. In reality, we've all glimpsed this hell and its myriad mini tortures ...

## i) Distributed monolith

One of the most fundamental principles of a microservice architecture is *independent deployment*. There are numerous benefits that result from this, but at the most basic level it's about simplification, speed and tiny increments.

So why - then - do we often insist on testing our whole suite of services in one huge gulp!

If you have to manage every single moving part in one massive test deployment then you no longer have a set of microservices but rather a painfully distributed monolith. Needless to say, this is going to be extremely difficult to consistent spin up, debug and develop for. Any activity at this level should really be kept to the few smoke or end-to-end tests that can verify your system.

Contract testing frameworks, such as [pact](http://pact.io), can be a saviour here.

## ii) Tending to special unicorns

Tests of any category should be simple to check-out, kick-off and develop/debug. However, far too often, it can involve a Herculeon effort of trial and error just to get the damn things running in the first place.

Wrestling with pages of undocumented configuration, rivaling the bulk of War & Peace, can make you want to quit before you've even begun.

Machines that need some special pampering and preening before they even think about playing ball inevitably result in a team fearful of touching 'the sacred server' in case they piss off the testing gods.

Those that cannot be easily executed from a development environment are doomed to a future of bit rot and ever increasing padding with inventive variations on "sleep(1000)" to get the build to "please just go green again!"

A consistent way to deploy and control a deployment is the antidote here. Tooling such as [arquillian](http://arquillian.org/) can given Java developers that piece of mind. Or alternatively, if you're able to encapsulate that "specialness" within a docker container you can use docker-compose (and friends) to orchestrate in a consistent way and move your tests in the right direction.

## iii) Too much tea

If, more often than not, when you run your tests you have enough time to make a(nother) cup of tea then there's a bit of an issue. Slow tests mean a far longer feedback loop, less pace and more importantly loss of focus.

It's understandable that for a certain small class of test - those that run end-to-end - will take their fair share of time. But every effort should be made to keep the bulk of your tests running lightening quick.

The greatest crime of all is using those dreaded blocking waits to control flow within a test. These stack up *really* quickly and breed even faster - one sleep usually leads to enough instability to require a number of peers. Tests should be strictly event driven, reacting directly to triggers in the system under test.
    

## iv) An unbalanced portfolio

We're all aware of the sacred 'test pyramid' demonstrating the range of tests from fast and cheap to slow and costly. There are many variations on this metaphor these days, but they are all based on the same principle: cover as much of your functionality using the lower levels of the pyramid and only extend further up the structure as needed to cover the rest.

Avoid covering functionality in your heavier testing layers when it can be taken care of by the simpler and snappier ones.

## v) Testing in paradise

Little in life is ever certain. However, although in the old monolith world you couldn't ever be completely confident in each and every action, you could be fairly sure a call between one module and another was pretty much guaranteed.

In the distributed systems domain of microservices very little is certain. If your system has a large enough footprint it is almost never going to 100% healthy. It's foolish to test and not take this into account, or to at least not mitigate these issues via more modern chaos engineering strategies.

Tools to check out in this space would include [saboteur](https://github.com/tomakehurst/saboteur), [hoverfly](https://hoverfly.io/) and [simian army](https://github.com/Netflix/SimianArmy/wiki) .

## vi) Thinking that testing ends when you ship code

Traditional testing strategy is very much focussed on 'the product'. You design it, develop it, run a whole bunch of tests (hopefully automated) before burning it on a disc and kicking it out the door.

Can't we take the same approach with a microservice deployment? Well many have certainly tried and inevitably struggled to make it succeed. The inherent complexity in a more advanced distributed system makes catering for every eventuality an enormous task that quickly becomes insurmountable. 

By throwing in the towel and stepping back it becomes clearer that testing is really a probability game. We want to get a near to 100% coverage as we can without spending all our resource on getting there.

A smarter investment may be to pick up much of low hanging fruit when it comes to traditional testing, and then investing the rest in improving our QA in production ability. Strategies such as:

* [**canary deployments**](https://octopus.com/docs/deployment-patterns/canary-deployments) - routing a small selection of users onto a fresh deployment to check its stability.
* [**shadowing**](http://blog.christianposta.com/microservices/advanced-traffic-shadowing-patterns-for-microservices-with-istio-service-mesh/) - duplicating traffic from the live environment onto a new deployment, and comparing the results to assert correctness.

This clearly comes with a certain amount of maturity within the services themselves. You cannot go about deploying potentially breaking changes into production without an ability to roll back easily, gracefully degrade and to provide alternatives/fallbacks in the case of an issue.

Definitely a goal worth pursuing.
