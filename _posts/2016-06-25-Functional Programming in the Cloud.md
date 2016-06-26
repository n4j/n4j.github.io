---
layout: post
title:  "Functional Programming in the Cloud"
date:   2016-06-25 14:19:10 -0500
categories: [AWS Lambda, Functional Programming, Cloud, Azure Functions]
keywords: "AWS Lambda, Functional Programming, Cloud, Azure Functions"
---
# Functions as a Service

Functional programming paradigm has been around for quite some time and its very popular in programming circles but its use had remain limited to academia and some very esoteric use cases. But recently, there has been a new found interest in this paradigm in Cloud space and every vendor either has an offering around it or slated to have one. In this post we will examine why functional paradigm is more relevant in cloud and how we can take benefit of it.

First let's see what functional programming is at 50,000 ft. Its a philosophy in which a method/function does not have side effect and does not alter the global state of an object. The functions which adhere to this philosophy are called pure functions and they are idempotent in a sense that they can be executed n number of times and there output will remain the same when same parameters are passed. Mathematically speaking
{% highlight mathml %}
f(x,y) => {z}
{% endhighlight %}

i.e. An output of a function f taking parameter x & y will always map to a set z. Some example of very popular modern functional programming languages are Haskell and Scala.

And how they are relevant to the Cloud? Ask any Cloud Sales Executive and they would reply (and I quote) `Better Scalability, Better Manageability, Keeping Operational Costs in Check` and some of the seasoned ones will also throw in buzz words like `Agility, HIPAA/NIST/FedRAMP Compliance, Better Line of Sight on IT infrastructure cost centers` etc. While these may sound as pure demagoguery, they do have some truth in them.

With **Functions As A Service or FaaS** as I like to call them (there, I threw in one of my one buzz words), you write  a function which is invoked every time a specific event occurs. Once the event is *serviced*, the underlying infrastructure on which it is executed *disappears*. Think about it in this way, there is a container inside which your code executes in response to an event and once that event is handled your container will be disposed of if there are no more events to handle. If multiple events occur at the same time then one container is created to handle each of the event and the Cloud will take care of requesting routing, load balancing and resource management. And what are the sources of these events? These events could stem from a range of sources like a Rest API call, a monitoring alert created on a resource like a VM, a message sent to a message queue, a database trigger, a scheduled task or a chron job.

Let's take an example of the recently concluded Brexit referendum. Say for example, all British citizens are required to either download an App or login to the British government website to cast there vote (authentication and singularity of each vote is left as exercise to the reader). If you are an infrastructure guy then you have already sweated over the load backend services for the App & Website will have to face. Now, assume that the logic of recording a vote is implemented as an AWS Lambda and an API is hooked up with it and every time some one would cast a vote a Lambda function would be executed and the vote will be recorded in AWS Dynamo DB or Azure Document DB. With this solution whole referendum process could virtually be completed without having to deploy a single server IaaS or PaaS for backend services. Sounds to good to be true, right? At the time of writing this none of the FaaS offering are infinitely scalable, they do have limits attached to them but you could get around them using throttling or via multi-region deployment.

And how is this beneficial in terms of all the "bilities"? With FaaS all you have to do is select a runtime (NodeJS, Python, etc.), amount of RAM and an execution timeout after which your code would be terminated. But isn't this similar to PaaS? No & Yes. No because you don't have to setup load balancing or worry about an instance failing due to resource quench etc. And yes because just like PaaS you need to select underlying runtime and some resource allocation.

But the major distinction over  here is, your FaaS container won't hang around forever if it has nothing to do and you will always have enough containers executing to service your load and your function executes only in response to an event, you can't start a server here and actively listen for requests.

Here's are some of the FaaS offerings:

* Amazon WebServices - [Lambda](http://docs.aws.amazon.com/lambda/latest/dg/welcome.html)
* IBM BlueMix - [OpenWhisk](https://developer.ibm.com/openwhisk/)
* Microsoft Azure - [Functions](https://azure.microsoft.com/en-us/services/functions/)
* auth0 - [Webtask](https://webtask.io/)

So fire up your favorite Cloud today and give functional paradigm a spin.
