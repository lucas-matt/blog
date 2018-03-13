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

Tests are clearly and extremely important port of a developer's day-to-day. They provide us with a safety net as we hack away at the rotten roots of our codebase, allowing us to feel more like a real engineer and less like we're walking through an antiques shop wearing a 12th century suit of armour.

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
    

## iv) An unbalanced portfolio


## v) Testing in paradise


## vi) Thinking that testing ends when you ship code


