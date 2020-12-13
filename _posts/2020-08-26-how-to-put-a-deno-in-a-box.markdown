---
layout: post
title: How to put a deno in a box (deploying with Docker, Heroku and Kubernetes)
date: 2020-08-26 13:32:20 +0100
description: Can deno fit into a container? # Add post description (optional)
img: deno_box.jpeg # Add image post (optional)
fig-caption: deno-box says Rooooar! # Add figcaption (optional)
tags: [javascript, deno, docker, kubernetes]
---
After a little vacation, I have started a new journey: “Can deno fit into a container?” The short answer is: yes!! and then you can read the rest of the trip exploring Docker, Heroku and Kubernetes.

You will remember me from my last article about deno: https://luisnavarro.eu/deno-goes-one-step-ahead/. So, I’m going to use that application to dive into the world of containers.


## Create a Docker image

![Deno in docker]({{site.image_url}}/20200826/deno_docker.png)

Currently, there isn’t any official Docker image with deno installed, but hayd has uploaded one that it’s close to being: https://hub.docker.com/r/hayd/deno So, this is the one that I’m going to use in my Dockerfile.

Next, expose the same port I’m using in my deno server app, use deno user for security, choose the necessary files carefully to run the application in a production environment and use only the needed parameters for the execution.

Running it and voila!

![It’s alive!!!!]({{site.image_url}}/20200826/deno_docker_running.png)

The biggest challenge making this container was to look for the minimal image to run deno and try to pass an environment variable as an argument into CMD to activate the debug mode for the logs. As it was impossible, I changed my approach and added into the code the functionality for reading an environment variable to activate it directly.

![new backward compatible functionality]({{site.image_url}}/20200826/debug_logs_docker.png)

## Deploy your Docker image in Heroku


![deno goes to Heroku]({{site.image_url}}/20200826/deno_heroku.png)

With an image ready to deploy in any environment I looked for one site to upload it and I chose Heroku because it’s well known and free for proofs of concepts.

Heroku CLI can make its own dyno container with the Dockerfile, but something went wrong, after deploying the port to expose was impossible to bind :(

I discovered that Heroku uses an environment variable ($PORT) to expose the port inside his network so I did some fixes in the code to take the port from an environment variable when it’s available.

![fixing deploy in Heroku]({{site.image_url}}/20200826/fix_deploy_heroku.png)

And now my application can be used through the internet :) Kudos for Heroku.

## Put your boxes in Kubernetes

![deno containers in Kubernetes: Roaaar!, Roaar!, Roaaar!]({{site.image_url}}/20200826/deno_kubernetes.png)

Now that the app is deployed in production… what happens if this is only a building block of your microservices platform? A good idea is to put all the pieces in a Kubernetes cluster. To make this simple I only have configured Kubernetes to scale my deno server application.

First thing to create a pod with our deno container is to publish our image to a registry server, you can use Docker Hub or create your own local registry server.

Once you have your image published you can create your first pod and run it. This is the easy way to do it: write where is your image and the port listening:

![a deno pod running]({{site.image_url}}/20200826/deno_pod_running.png)

If you don’t want to do the port-forward you can create a service to expose the port where you want to access your application. So add a label to your pod to connect them:

![a deno pod with a service running]({{site.image_url}}/20200826/kubernetes_service.png)

At this point it’s a good practice to add some devops stuff as a readinessProbe to check when your app is ready to accept requests, a livenessProbe to check if your app is still running and some resources to limit the amount of cpu and memory that the pod can consume.

Next step is to create a deployment set to scale your application so I put together a ConfigMap for passing environment variables to the app, a Service to expose the port for receiving requests and a ReplicaSet with 3 deno servers so when one is down another one is going to be launched automatically.

![a deno deployment running]({{site.image_url}}/20200826/kubernetes_deployment.png)

If I try to delete a pod another one is launched automatically.

![the deno pod phoenix is here again]({{site.image_url}}/20200826/pod_phoenix.png)

The most challenging stuff with Kubernetes has been to understand how all the pieces work together and select which ones are going to be useful and which ones are not necessary to keep it as simple as possible. Kubernetes compared to Heroku is more difficult to manage and to learn, so you are going to spend some time mastering it if you want to be successful in a production environment.