---
title: "Python 3 Asyncio"
tags: ["python", "async"] 
thumbnail: /images/python-async/python.jpg
category: Programming
description: "A quick introduction to Python programming with async/await"
date: 2017-08-09 00:00:00
---

![](/images/python-async/python.jpg)
<div style="text-align: right"><sub><sup>["Carpet Python"](https://www.flickr.com/photos/29880452@N05/2819784303/) ([CC BY-NC 2.0](https://creativecommons.org/licenses/by-nc/2.0/)) by [exhibitionwhitmore](https://www.flickr.com/people/29880452@N05/)</sup></sub></div>

---

Asynchronous programming has become a core practice over the last decade. *One thread per request* architectures have disappeared for the most part and have been replaced by non-blocking functionality, whether a *Servlet 3* style @Suspend, NIO actor support via *Akka* or coding at a lower level against the *Netty* framework. Of course there are many advantages to asynchronous programming, most centered around resource efficiency and avoiding CPU context switches - but that's a topic that has already been very well covered, so I'll avoid a detour right now.

Python has had some great asynchronous options for a long time. *Gevent*, *Twisted* and *Tornado* have all had, and continue to have, a great community and support. Each being a python library they have a certain opinionated approach that is somewhat incompatible with the others. For example, *Tornado* is specifically targeted at web application programming whereas *Gevent* and *Twisted* are more generally concurrency libraries for interleaving non-blocking 'threads'.

Enter Python 3 Async/Await - we've entered some exciting times! Python 3 seems to finally be gaining the traction it deserves to help it overthrow the long standing Python 2 dominance. One major evolution that has been added fairly recently (late 2015) is the native asyncio library that gives us coroutine support directly within the core language.

Let's spend some time checking out examples of this exciting new feature, but first - what the heck are coroutines?

# Programming with Coroutines

So what are coroutines? They're not something I'd ever come across in the Java world, and they take a little getting used to. Here's the formal definition:

{% blockquote Wikipedia https://en.wikipedia.org/wiki/Coroutine %}
Coroutines are computer program components that generalize subroutines for non-preemptive multitasking, by allowing multiple entry points for suspending and resuming execution at certain locations. Coroutines are well-suited for implementing more familiar program components such as cooperative tasks, exceptions, event loops, iterators, infinite lists and pipes.
{% endblockquote %}

Umm, ... so ... did that make sense? I gather that it's something about pausing and resuming execution, but that's about it.

Let's try a 

![](/images/python-async/takeaway.jpg)
<div style="text-align: right"><sub><sup>["Takeaway"](https://www.flickr.com/photos/126337928@N05/23964405150/) ([CC BY 2.0](https://creativecommons.org/licenses/by/2.0/)) by [Dai Lygad](https://www.flickr.com/people/126337928@N05/)</sup></sub></div>


# Hello World

```
import asyncio

async def hey():
    print("Hello World!")

loop = asyncio.get_event_loop()
loop.run_until_complete(hey())
loop.close()

```

```
import asyncio

async def excl():
    return "!"

async def world():
    return "World" + await excl()

async def hello():
    print("Hello " + await world())

loop = asyncio.get_event_loop()
loop.run_until_complete(hello())
loop.close()
```


# Fibonacci Time

```
import asyncio

async def fibn(n):
    if n == 0:
        return 0
    if n == 1:
        return 1
    return await fibn(n-2) + await fibn(n-1)

loop = asyncio.get_event_loop()
c = loop.run_until_complete(asyncio.gather(fibn(5), fibn(6), fibn(7)))
print(c)
loop.close()
```

# Async Decorators


```
import asyncio

def memo(fn):
    cache = {}
    async def wrap(n):
        if n in cache:
            print("found %s in cache" % (n))
            return cache[n]
        res = await fn(n)
        print("calculated %s" % (n))
        cache[n]= res
        return res
    return wrap

@memo
async def fibn(n):
    if n == 0:
        return 0
    if n == 1:
        return 1
    return await fibn(n-2) + await fibn(n-1)


loop = asyncio.get_event_loop()
c = loop.run_until_complete(asyncio.gather(fibn(4), fibn(4), fibn(4)))
print(c)
loop.close()
```

# But Who Cares About Async Fibonacci!? 
