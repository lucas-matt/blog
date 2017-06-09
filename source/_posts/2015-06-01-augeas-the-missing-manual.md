---
layout: post
title: Augeas the Missing Manual
description: "A guide for some JSON processing using the Augeas configuration tool"
date: 2015-06-01
tags: [augeas]
cover: /images/abstract-1.jpg

---

![](/images/augeas.jpg)

Lately I've been using Augeas to configure some json files, and phew! it's not been easy! So here's a little guide to help other unfortunate souls lost in the Augeas wilderness.

1) First let's start with a pretty basic json file located at <strong>/tmp/test.json</strong>:

{% codeblock lang:javascript %}
{ "a": 1 }
{% endcodeblock %}

2) <strong>Load</strong>

   Loading the file with the json lens is easy enough once you know how
{% codeblock lang:plain %}
augtool> set /augeas/load/Json/lens Json.lns
augtool> set /augeas/load/Json/incl /tmp/test.json
augtool> load
augtool> print /files/tmp/test.json

/files/tmp/test.json
/files/tmp/test.json/dict
/files/tmp/test.json/dict/entry = "a"
/files/tmp/test.json/dict/entry/number = "1"
{% endcodeblock %}
3) <strong>Set</strong>

   As is changing the existing property
{% codeblock lang:plain %}
augtool> set /files/tmp/test.json/dict/entry[. = "a"]/number 2
augtool> save
Saved 1 file(s)
{% endcodeblock %}
  resulting in
{% codeblock lang:javascript %}
{ "a" : 2 }
{% endcodeblock %}
4) <strong>Set Type</strong>

   Of course you can change this to a string or boolean property as required
{% codeblock lang:plain %}
augtool> rm /files/tmp/test.json/dict/entry[. = "a"]/number
augtool> set /files/tmp/test.json/dict/entry[. = "a"]/string hello
augtool> save
Saved 1 files(s)
{% endcodeblock %}
   to create
{% codeblock lang:javascript %}
{ "a": "hello" }
{% endcodeblock %}
5) <strong>Add Sub-object</strong>

   And finally adding some subobjects with properties
{% codeblock lang:plain %}
set /files/tmp/test.json/dict/entry[. = "b"] b
set /files/tmp/test.json/dict/entry[. = "b"]/dict/entry[. = "c"] c
set /files/tmp/test.json/dict/entry[. = "b"]/dict/entry[. = "c"]/dict/entry[. = "d"] d
set /files/tmp/test.json/dict/entry[. = "b"]/dict/entry[. = "c"]/dict/entry[. = "d"]/string "world"
augtool> save
Saved 1 files(s)
{% endcodeblock %}
   to create an json structure with depth:
{% codeblock lang:plain %}
{
    "a": "hello",
    "b": {
        "c": {
            "d": "world"
        }
    }
}
{% endcodeblock %}
