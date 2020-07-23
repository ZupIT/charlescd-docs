---
description: This section describes how you may configure our Circle Matcher inside Charles
---

# Circle Matcher

## Why do I have to configure Circle Matcher?

Since Circle Matcher is one of our modules, you might ask why do I have to configure it inside Charles? Well, Circle Matcher was designed to be a standalone application that would receive the most part of requests in Charles environment \(because it is the application that identifies the user based on the rules that you configured while managing a circle\) you may want to install this specific module of Charles' stack anywhere that you might feel more comfortable with \(in a public cluster, for example\).

## What do I have to configure?

While defining a workspace, you may want to configure a Circle Matcher to perform operations inside Charles like creating a circle and editing segments in a circle. When Circle Matcher is configured and you already created your circles, you can then start deploying in circles with Charles.

When creating a workspace, you can tell Charles for which Circle Matcher that current workspace will point to. Since Charles can handle multiple environments in multiple workspaces, you can have one Circle Matcher for each environment. 

Basically, you configure the public DNS that points to your desired Circle Matcher, for example **http://charles.info.example/charlescd-circle-matcher**.

If you want to use Circle Matcher in the same namespace that Charles is installed, you can use DNS referring too but in terms of performance, is better if you use Kubernetes service name, like **http://charlescd-circle-matcher:8080**.



