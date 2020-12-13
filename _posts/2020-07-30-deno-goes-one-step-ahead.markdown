---
layout: post
title: Deno goes one step ahead
date: 2020-07-30 13:32:20 +0100
description: Doing a configuration file server to enable features. # Add post description (optional)
img: dino.jpg # Add image post (optional)
fig-caption: Deno is ready to walk alone # Add figcaption (optional)
tags: [javascript, deno]
---
### Doing a configuration file server to enable features

This is not an article about if Deno is better than Node or how to start with Deno (although you can follow this article if this is your first time with Deno). This is an article about what things you are going to deal with if you want to do a project with Deno (my own experience).

Let’s go ahead! I wanted to do a PoC about a server that could return a configuration file to enable features by environment (dev, test, acceptance, production) and device (web, android, ios,…) in an easy way without a database and which I can put in any site (aws, azure, gcp) and accept hundreds of request per second.

My first idea was using node with express (I had used in the past with well results) but I remembered a friend of mine talking about Deno replacing Node… and I said: Why not?

First thing I did was to open Deno documentation (https://deno.land/manual/introduction) and install deno. It was a nice surprise that npm is not used (wheeee!) so the installation was super fast and I was coding in VSC in minutes after installing Deno plugin.

![Deno plugin]({{site.image_url}}/20200730/deno_plugin.png)

Minutes later I discover that when I change a file the server doesn’t restart immediately so after a little of research I discover denon (https://deno.land/x/denon). Perfect. Coding is faster now.

![denon]({{site.image_url}}/20200730/denon.png)

Another thing that I want to do while I’m programming is debugging. I was be able to do it in VSC after configuring launch.json in .vscode with something like this:

![Debugging deno with VSC]({{site.image_url}}/20200730/debugging_deno.png)

One of the theoretically good things about Deno is security since you have to give access to read or write files, access to network, … That’s ok but when you are coding to make it more simple you run it with -A to give all permissions as they are very common in any project. (Yeah, when you are going to deploy to a different environment it’s a good idea to only give the permissions that you are using).

The test suite has a good feature that can be a pain in the ass: The one that checks if you are waiting for all the promises you’ve launched. In one month I’ve got a lot of problems with this each time there is an update on Deno or some other library. So final solution was not to update if is not needed using always the same versions. When you are in a hurry and one update breaks your projects that makes you cry, so update them when you have time to do it (it’s a good practice) but don’t let that an update kills you.

Another issue that I’ve found with the test suite, maybe for the server library that I chose (http-wrapper), is that the server is not killed when the tests finish, so integrating this in a pipeline could be difficult. There is a good thing about this, if you change the test, it runs automatically. Every cloud has a silver lining.

A good tip for testing is installing a rest client plugin in VSC to have a bunch of requests ready to launch like REST Client from Huachao Mao.

![REST Client plugin]({{site.image_url}}/20200730/rest_client.png)

Logging is ok but could be better if the logging configuration was in a config file or something like that (I’m very used to log4j). For example if I want to enable debug logs I run the server with a “debug” argument and change the default handler and logger.

![debug logs configuration]({{site.image_url}}/20200730/debug_logs.png)

Benchmarking is something that I love. Using it is really easy and efficient. You can take metrics of the important things with only some lines of code and then do load tests super fast without the need of using other tools.

![benchmarking with deno]({{site.image_url}}/20200730/benchmarking.png)

Monitoring and alerting are missed and there is not support for Opentracing, Prometheus or Grafana. 

Summarising: Deno is more strict than node so this is going to be better for security and for quality code. Coding is fast and you can create in a pair of days a server that can deal with hundreds of requests per second. So it’s perfect for a personal project, an internal project or a startup. If you are looking for something more stable it’s better to choose Node or Go.

I have enjoyed a lot doing this project (https://github.com/fundyc/deno_server) and I only want to share to everybody what I have suffered or loved doing something new. I expect that you have had fun riding this journey.