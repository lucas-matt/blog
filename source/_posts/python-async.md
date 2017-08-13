---
title: Python 3 Asyncio
tags:
  - python
  - async
thumbnail: /images/python-async/python.jpg
category: Programming
description: A quick introduction to Python programming with async/await
date: 2017-08-13 00:00:00
---


![](/images/python-async/python.jpg)
<div style="text-align: right"><sub><sup>["Carpet Python"](https://www.flickr.com/photos/29880452@N05/2819784303/) ([CC BY-NC 2.0](https://creativecommons.org/licenses/by-nc/2.0/)) by [exhibitionwhitmore](https://www.flickr.com/people/29880452@N05/)</sup></sub></div>

---

Asynchronous programming has become a core practice over the last decade. *One thread per request* architectures have disappeared for the most part and have been replaced by non-blocking functionality, whether a *Servlet 3* style @Suspend, NIO actor support via *Akka* or coding at a lower level against the *Netty* framework. Of course there are many advantages to asynchronous programming, most centered around resource efficiency and avoiding CPU context switches - but that's a topic that has already been very well covered many times over, so I'll avoid a detour right now.

Python has had some great asynchronous options for a long time. *Gevent*, *Twisted* and *Tornado* have all had, and continue to have, a great community to support them. Each being a standalone library they have a certain opinionated approach that is somewhat incompatible with the others. For example, *Tornado* is specifically targeted at web application programming whereas *Gevent* and *Twisted* are more general concurrency libraries for interleaving non-blocking 'threads'.

Enter `async`/`await`. Python 3 seems to finally be gaining the traction it deserves to help it overthrow the long standing Python 2 dominance. One major evolution that has been added fairly recently (late 2015) is the native asyncio library that gives us coroutine support directly within the core language.

Let's spend a little time checking out examples of this exciting new feature, but first - what the heck are coroutines?

# Programming with Coroutines

So what are coroutines? They're not something I'd ever come across in the Java world, and they take a little getting used to. Here's the formal definition:

{% blockquote Wikipedia https://en.wikipedia.org/wiki/Coroutine %}
Coroutines are computer program components that generalize subroutines for non-preemptive multitasking, by allowing multiple entry points for suspending and resuming execution at certain locations. Coroutines are well-suited for implementing more familiar program components such as cooperative tasks, exceptions, event loops, iterators, infinite lists and pipes.
{% endblockquote %}

Umm, ... so ... did that make sense? I gather that it's something about pausing and resuming execution, but that's about it.

Let's try a real world analogy - picking up some dinner from your favourite takeaway.

![](/images/python-async/takeaway.jpg)
<div style="text-align: right"><sub><sup>["Takeaway"](https://www.flickr.com/photos/126337928@N05/23964405150/) ([CC BY 2.0](https://creativecommons.org/licenses/by/2.0/)) by [Dai Lygad](https://www.flickr.com/people/126337928@N05/)</sup></sub></div>

Arriving at the takeaway you politely join the queue of customers (the coroutines) waiting in line to get served by the cashier. For simplicity's sake, let's assume there is only one cashier able to serve you. This person represents the main process, or event loop.

Each customer is served by the cashier one at a time. He/she takes your order, money, and gives you a little ticket representing the meal being prepared behind the scenes. The process of cooking your food represents external I/O, such as a network call or filesystem access.

Now, in a blocking I/O world, you and the cashier would awkwardly eyeball each other for the full length of time it took to cook your food - the other customers looking on in frustration. By using coroutines, rather than keeping the poor cashier occupied, you put your interaction on hold and go wait quietly in the corner so that another customer can be served in the meantime. When your food is ready to go the cashier is notified by the kitchen and resumes your interaction to complete the process.

In a similar way coroutines yield their control of the main thread when they encounter a blocking operation and then resume execution once that operation completes. This allows for the core process to remain active as long as there is valuable work to be done, rather than blocking important tasks whilst waiting on external factors.

Now for some specific examples.

# Hello World of Asyncio

Let's start off with the obligatory 'hello world'.

```
import asyncio

async def hey():
    print("Hello World!")

loop = asyncio.get_event_loop()
loop.run_until_complete(hey())
loop.close()

```

The function itself is pretty mundane although it is marked with the `async` keyword to denote its nature. To get access to the event loop to execute this special type of routine, we grab it from asyncio and ask it to run our coroutine to completion.

The example below is slightly more interesting, showing how coroutines can interact with and utilize one another. 

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

Here we are introduced to the `await` keyword. This allows one coroutine to yield its program flow until we get the result from another asynchronous operation, at which point we can resume operation. Rather simply, here we are just chaining the invocation and resolution of a series of coroutines.

# Fibonacci Time and Async Decorators

Now for something a bit meatier - recursion and decoration!

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

First, take a look at the `fibn` function. This is a good old recursive fibonacci, however the delegate calls are made asynchronously. Of course, this gives no real benefit, but does prove we can invoke coroutines in a recursive fashion. 

More interestingly we have implemented a memoizing decorator that will cache the result of an asynchronous call. We have to be careful to declare the wrapper function (as created within the decorator) as `async` so that we can `await` completion of the decorated function. 


# But Who Cares About Async Fibonacci!? 

Playing with these toy functions is all very well, but what bearing does this have on helping us solve real world problems?

TBH, the list of libraries supporting `async`/`await` functionality right now (at least at the time of writing) seems to be fairly limited. Obviously the intention is that over time this will become a much more richly supported and standardized way of writing asynchronous code with the core python language. Although, having said that, as long as you're using fairly common technology you'll likely find at least one driver that supports this approach.

In this final example we can see how easy it is to wire up an asynchronous application end-to-end - a REST service that reads and writes asynchronously to mongodb.

```
import json
from aiohttp import web
from motor.motor_asyncio import AsyncIOMotorClient

client = AsyncIOMotorClient('mongodb://mongodb:27017')
db = client.demo

async def retrieve(req):
    id = req.match_info.get('id')
    result = await db.stuff.find_one({'_id': id})
    if result:
        return web.Response(body=json.dumps(result))
    return web.Response(status=404)

async def save(req):
    id = req.match_info.get('id')
    json = await req.json()
    json['_id'] = id
    result = await db.stuff.replace_one({'_id': id}, json, upsert=True)
    status = 200 if result.matched_count >= 1 else 201
    return web.Response(status=status)

app = web.Application()
app.router.add_get('/stuff/{id}', retrieve)
app.router.add_put('/stuff/{id}', save)
web.run_app(app)
```

There are a few notable points in this small application.

* `[line 8, 15]` First we have the handling of requests themselves. Our handlers are specified as coroutines so that the HTTP server doesn't have to block a thread whilst waiting for the end-to-end response to complete.
* `[line 17]` Even trivial I/O, such as reading the request payload from the socket, is handled in an efficient way.
* `[line 10, 19]` As expected, any database access is handled in a non-blocking fashion.

So pretty simple stuff, but it all seems to fit together in a sensible and consistent manner. It'll be interesting to see how this feature of python evolves both within the language and across the ecosystem over the next few years.

Happy non-blocking!
