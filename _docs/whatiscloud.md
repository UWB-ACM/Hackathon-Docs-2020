---
layout: default
category: Getting Started
title: Cloud Services FAQ
order: 1
permalink: /clsvcfaq.html
---

# Cloud Services FAQ

If you're new to the idea of working with the cloud and you want a quick and easy primer to get you up and running, you're in the right place. This page will cover what the cloud is, how it works, and why you should care.

### So what's the cloud anyway?
Quite simply, the cloud is a way to run your software on someone else's machines. In most cases, developing software for the cloud is just like developing software for running locally. This allows you to build complex online applications using the same programming languages and concepts you’re already familiar with while avoiding the hassle of provisioning and maintaining infrastructure.

There are two kinds of offerings that you might see when browsing a cloud provider’s website. Briefly, these are **Virtual Machines** and **Services**.

* Virtual Machines, or VMs, are exactly what they sound like. They’re a virtual environment that emulates a real computer, hosting an Operating System and any software you might want to run. For example, when using a VM as a server, you need to setup packages, configure settings, and run your server application manually, just as you would if you setup your own computer as a server. Cloud VM offerings include Amazon’s AWS EC2 and Azure’s VMs. VMs won’t be covered in detail here. For now, just know that they exist and what they’re for.

* Services, on the other hand, abstract away the nitty-gritty implementation details. Services allow you to build and deploy your application without worrying about the underlying details. For example, Amazon’s AWS Lambda service allows you to deploy functions that contain regular code. AWS might run your code in a VM, or in a Container. The key thing to remember here is, *it doesn’t matter to you*. Most cloud offerings fall into the Services category, and they make developing cloud applications much easier than it might appear.

### Intriguing...so what can I do in the cloud?
Almost anything you can dream of. Cloud providers are constantly improving and adding to their offerings. A good place to start would be to take a simple application that you could run locally, such as an app that retrieves and stores information in a database and move it into the cloud. Refer to [our collection of example projects]({% link _docs/project-ideas.md %}) for a few concrete examples.

### Wait, there's more than one cloud provider?
You bet. Amazon Web Services (AWS), Microsoft Azure, and Google Cloud Platform (GCP) are the three most well-known cloud providers out there today, but they aren’t the only ones. In order to keep these docs coherent (and us, sane) we’ll be focusing on these three.


### Why should I care about the cloud? It works fine on my machine!
Try shipping your machine to the customer, then get back to me. Really though, it’s because the applications that you can create in the cloud can be far more capable than what you can create on your machine. Web hosting, machine learning, databases, serverless functions and more are at your fingertips allowing you to build applications in a way you never could before.

### Alright, you've convinced me. So how do I get started?
Head to [our guide on cloud providers](_docs/cloud_account_setup.md) and follow the exquisitely detailed instructions to learn how to signup for a free account and get free student credits on each cloud platform. It is highly recommended to peruse the documentation for each offering you want to use. Links to AWS, Azure, and GCP documentation websites have been provided for your convenience.
* [AWS Documentation](https://docs.aws.amazon.com/index.html)
* [Azure Documentation](https://docs.microsoft.com/en-us/azure/)
* [GCP Documentation](https://cloud.google.com/docs/)

### Is this stuff really free???
There is no such thing as a free lunch. More to the point though, each provider has a set of “always free” offerings, and a set of free trial offerings. All free offerings have usage limitations which will be clearly marked on the provider’s website. **Exceeding the listed limitations will incur charges which you will be responsible for.** Cloud providers will email you when you use credits or have a new invoice posted. Keep track of these communications and avoid leaving services running in order to minimize potential charges. Free trials typically last 12 months, after which you will be charged for any offering that is not in the “always free” category. Student accounts can often be renewed at the provider’s discretion.
