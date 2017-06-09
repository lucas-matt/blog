---
layout: post
title: "Python Development with Vagrant & IntelliJ"
description: "A quick guide to making Python development even easier by using Vagrant and IntelliJ"
date: 2016-08-24
tags: [python, intellij, vagrant]
cover: /images/abstract-5.jpg
---

![](/images/pyintellij/header.jpg)

I'm mostly a JVM language developer who loves the toolset that comes from the JVM world, especially that of a good IDE. Don't get me wrong I'm partial to a bit of emacs, but I spend most of my time, these days, staring into the awesome Darcula black of IntelliJ.

So when it comes to developing a bit of Python I want to try to keep my foundations as comfortable as possible without compromising the new environment. More specifically this comes from developing a Linux-based Python app whilst on a Windows machine. With the power of Python stemming from its C-based libraries you lose some of that platform independence that you otherwise come to take for granted.

Enter Vagrant (let's ignore Otto for now). Vagrant enables users to create and configure lightweight, reproducible, and portable development environments. This is great for development targeted at one platform whilst coding on another - a flexible sandbox that you can build up and and tear apart with the execution of a single command.

Also, helpfully, IntelliJ has some great bindings to allow us to develop with ease on a Vagrant environment. This is what we will quickly explore.

## vagrant up

To get started with Vagrant you just need to have it and VirtualBox (or similar) installed. Then a simple configuration Vagrantfile is the only hard work we need to put in. Here's one I made earlier:

{% codeblock lang:plain %}
Vagrant.configure(2) do |config|

  config.vm.box = "bento/centos-6.7"

  config.vm.network "forwarded_port", guest: 8080, host: 8080

  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.name = "Intellij Vagrant Test"
  end

  config.vm.provision "shell", path: "setup.sh"

end
{% endcodeblock %}

Nothing much to it: just pick an image, forward some ports, change a little VM config and run the setup script once the environment has booted.

All that is in the setup.sh script is a little work to smooth out Python development a little.

{% codeblock lang:plain %}
rpm -iUvh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
yum -y install python python-pip gcc python-devel git
pip install --upgrade setuptools
pip install gevent
{% endcodeblock %}

Now just we run vagrant up, make some tea, and we're in!

<img src="/images/pyintellij/vagrant_up.png" width="500"/>

## Hooking in IntelliJ

Connecting up IntelliJ is actually pretty simple too. Just create a new project as usual and when the time comes create a new "Remote Interpreter".

<img src="/images/pyintellij/remote_interpreter.png" width="500"/>

Choose the project base (the folder containing Vagrantfile) as your Vagrant Host URL and the rest should be done automatically.

Once hooked in we can see details about our guest environment, such as the libraries that we have installed. To do this specifically navigate to Tools -> Manage Python Packages

<img src="/images/pyintellij/manage_packages.png" width="600"/>

## Throwing Together a Quick App

Now let's build a very small gevent wsgi app to demonstrate how the environment works - we'll just create a very lightweight "hello world" service.

First thing, setting up the tests. As we haven't used this environment before, some of the supporting tools, mocking frameworks, etc, will be missing from the environment. That's ok, IntelliJ makes it easy to just highlight the red parts of your code and automatically install those libraries. Check it out below with the mock framework.

<img src="/images/pyintellij/install_mock.png" width="400"/>

Now for some example test code:

{% codeblock lang:python %}
import unittest
import mock
import hello.server

class TestServer(unittest.TestCase):

    def test_default_response(self):
        response = hello.server.hello_world({}, mock.MagicMock())
        output = next(response)
        self.assertEqual("<h1>Hello World!</h1>", output)
{% endcodeblock %}

To run the test is no different than usual. Just kick off as a unit test and the remote interpreter we configured earlier will take care of the details of running the code "remotely".

Of course, our test fails, as it should, at this point

<img src="/images/pyintellij/failing_test.png" width="600"/>

Once we've developed our wsgi handler the test then passes.

{% codeblock lang:python %}
from gevent import pywsgi

def hello_world(environ, start_response):
    start_response('200 OK', [('Content-Type', 'text-html')])
    yield "<h1>Hello World!</h1>"

server = pywsgi.WSGIServer(
    ('', 8080), hello_world
)

if __name__ == "__main__":
    server.serve_forever()

{% endcodeblock %}

Finally, what about running the application and debugging it? - again, IntelliJ does a great job of hiding all those abstractions.

Just run/debug a program as you usually would, and thanks to our port mapping we can play around with the app as if it was running natively on the host rather than in a Linux VM.

<img src="/images/pyintellij/final_result.png" width="600"/>

## Links

* [IntelliJ Vagrant](https://www.jetbrains.com/help/idea/2016.2/vagrant-2.html)
* [Configuring Remote Python Interpreters](https://www.jetbrains.com/help/pycharm/2016.1/configuring-remote-python-interpreters.html)

