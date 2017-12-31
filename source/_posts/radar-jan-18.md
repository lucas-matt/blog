---
title: "Technology Radar Jan '18"
tags:
  - radar
  - growth
  - thinking
thumbnail: /images/radar-jan-18/firework.jpg
category: Tech Radar
description: Personal technology radar for Jan '18
date: 2018-01-01 00:00:00
---

![](/images/radar-jan-18/firework.jpg)
<div style="text-align: right"><sub><sup>["Firework"](https://www.flickr.com/photos/spi/3702243785/) ([CC BY-SA 2.0](https://creativecommons.org/licenses/by-sa/2.0/)) by [spi516](https://www.flickr.com/people/spi/)</sup></sub></div>

The [Thoughtworks Technology Radar](https://www.thoughtworks.com/radar) should be a familiar sight to any active technologist. The breadth of coverage across hundreds of interesting technologies, both new and old, has made its release something of an event in the software developer's calendar.

In an interesting [post](https://www.thoughtworks.com/insights/blog/build-your-own-technology-radar) by one of Thoughtworks key employees - Neal Ford - it is suggested that not only should enterprises produce their own version of the radar, but so should each individual software developer.

{% blockquote "Build Your Own Technology Radar" https://www.thoughtworks.com/insights/blog/build-your-own-technology-radar %}
You need two radars: one for yourself, to help guide your career decisions, and one for your company. 
{% endblockquote %}

In an attempt capture the direction of my career and hobbyist efforts I have taken this advice and produced my first personal radar.

# TL;DR

First a quick overview of my "Intro to 2018" list, which seems to be split into three broad categories.

**Near Term Increments**
Items related to Java, microservices and containerization would fall under this category. Technologies that I use on a day to day basis, and in which I should be fairly fluent, whilst continuing to improve my understanding.

**Longer Term Investments**
Technologies that are more aspirational in scope - e.g. AI and machine learning topics - but are likely to become much more prevalent in the industry within the next 5 years or so.

**Skill Diversification**
Skills that would be considered a "parallel path" to my everyday, but of which I should have some decent appreciation of - even if only at a very basic level. For example, Android or Alexa development.

Now for the radar in full.


# Techniques & Theory

![](/images/radar-jan-18/techniques.png)

### Too Many Cucumbers!
BDD, and Cucumber most specifically, are great tools when used in the right context. However having way too many tests, or generating unfocussed tests, can result in a slow, brittle and unmaintainable test suite. In addition to this, not having a key business stakeholder involved with the definition and understanding of the scenarios defeats the point somewhat. I'll be personally looking to use this technique/tool more judiciously in future.

### Deep Learning
Deep learning, and related techniques, have made neural networks cool again! Not to jump on any kind of bandwagon, but I'll be looking to further understand this class of algorithm more clearly, as I believe they will become much more prevalent in the daily life of a software developer in the upcoming decade and beyond.

### Service Mesh
A microservice architecture, although in the singular quite simple, in the aggregate can become quite an unmanagable behemoth. Managing communication, security, monitoring and other similar concerns that are orthogonal to the core business can form a significant part of this complex distributed system. Service meshes look to externalize and manage much of this trickery as a third part concern, allowing you to focus solely on what you do best.

### Chaos Engineering
If things can go wrong they usually will. Add to this fact that microservices up the surface area of things that can go wrong (specifically in between those services), then writing robust, resilient and scalable systems means we must probe at the corners of possibilities. A mature development team should be looking at chaos engineering practices to try and tease out any problematic edge cases before they occur in the wild.

### Lightweight Architecture Decision Records
Ever wonder why a decision was made or the context in which it was made. Lightweight architecture decision records are a technique for capturing those important decisions in a simple text format able to be kept in SCM alongside the sourcecode itself.

### Consumer Driven Contract Testing
An essential part of the microservice testing toolkit. They enable independent service deployments whilst maintaining that solid contract between any two neighbouring services.

### Reinforcement Learning
A relatively old machine learning technique that utilizes a feedback loop to train a software agent in a specific, realtime, environment. More recently, when combined with deep learning, has shown some very interesting applications - e.g. the famous Deep Mind Atari playing agents.

### Data Structures & Algorithms
As a software developer, unless you're very lucky, you spend much of your time putting things into databases and then pulling them out again in the future to show to an end user. As you can imagine, this doesn't involve a ton of good old Computer Science, and inevitably the knowledge of those core algorithms slowly diminishes. This is a "note to self" to visit some of those topics, and keep that knowledge fresh.

### Statistics
The continued rise of machine learning and data science require a good understanding of statistical methods and similar such topics. This is likely to become even more true as the field expands into every day software development.

### Linear Algebra
Machine learning algorithms are very much based upon core linear algebra concepts. To truely understand the magic inside some of those algorithms, a good understand of linear algebra is essential.

### Domain Driven Design
Designing microservices that are internally cohesive but well decoupled can be quite a challenge to impllement, as anyone who has worked on such a system will understand. An understanding of DDD is key to to ensure that the right design decisions are made.

# Tools & Frameworks

![](/images/radar-jan-18/tools.png)

### Tensorflow
TensorFlow is an open-source software library for dataflow programming across a range of tasks. It is a symbolic math library, and also used for machine learning applications such as neural networks.

### Scikit Learn
Scikit-learn is a free software machine learning library. It features various classification, regression and clustering algorithms including support vector machines, random forests, gradient boosting, k-means and DBSCAN, and is designed to interoperate with the Python numerical and scientific libraries NumPy and SciPy.

### Keras
Keras is a high-level interface in Python for building neural networks. Keras is open source and runs on top of either TensorFlow or Theano. It provides an amazingly simple interface for creating powerful deep-learning algorithms to train on CPUs or GPUs

### Jupyter
Increased interest in machine learning — along with the emergence of Python as the programming language of choice for practitioners in this field — has focused particular attention on Python notebooks, and the most popular of these is Jupyter.

### Open Tracing
A vendor-neutral open standard for distributed tracing

### Apache Spark
Apache Spark™ is a fast and general engine for large-scale data processing. Although still an excellen tool in the right circumstance, I've decided to focus my attention on learning less large-scale data science frameworks.

### Pandas
pandas is an open source library providing high-performance, easy-to-use data structures and data analysis tools for the Python programming language.

### Numpy
NumPy is the fundamental package for scientific computing with Python.

# Platforms

![](/images/radar-jan-18/platforms.png)

### AWS
Clearly AWS isn't anything new, but I've personally neglected getting more involved with the platform for way too long. Here's a "note to self" to dig in and understand it a little more.

### Serverless
The use of serverless architecture has very quickly become an accepted approach for organizations deploying cloud applications, with a plethora of choices available for deployment.

### Android
In a similar vein to my AWS entry, Android (or mobile development in general) should be understood at a basic level by any mature software developer - another todo for personal projects. 

### Alexa
Voice platforms are high in popularity at the moment. Whether a fad, or real trend, it's worth getting stuck in to a personal project on the Alexa platform for some diversification of skills.

### Sage Maker
Amazon SageMaker is a fully-managed service that enables developers and data scientists to quickly and easily build, train, and deploy machine learning models at any scale. Amazon SageMaker removes all the barriers that typically slow down developers who want to use machine learning.

### Kaggle
Kaggle is a platform for predictive modelling and analytics competitions in which statisticians and data miners compete to produce the best models for predicting and describing the datasets uploaded by companies and users.

### Kubernetes
Kubernetes is an open-source system for automating deployment, scaling, and management of containerized applications.

### Docker
Docker is the world's leading containerization platform - not much to add myself, just that it's currenlty a must have for serverside development.

# Languages

![](/images/radar-jan-18/languages.png)

### Kotlin
Kotlin is a statically typed programming language for modern multiplatform applications. It has overtaken Scala as my personal JVM alt-language of interest, and I'm looking to play with it a fair bit more this year.

### Python 2
Python 2 has been king for as long as I can remember, but with Python 3 getting some great new features and wider community support, it has finally come time for its abdication.

### Python 3
See Python 2

### Scala
For many years after my initial Clojure obession I saw Scala as my functional saviour from a frustrating and verbose Java. Despite many fun times with the language, its inherent complexity and (seeming) drop off in adoption have lead me to consider the growing alternatives - see Kotlin.

### R
A great data science and machine learning language. However, my larger personal experience with Python, and Python's continued dominence in the data science community, have led me to trim down my options and focus on a deeper rather than broader personal research.

### Effective Java 8/9
Java has (fairly) recently obtained some significant improves to the core language. However, as with everything, addition adds complexity. Some best practices around these features is key, and where better to study and in Joshua Bloch's revised book.
