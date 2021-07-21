---
description: 'In this section, you will find more information about metrics'' actions.'
---

# Metrics actions

## What is it? 

After you had registered your metrics group, Charles follows up and offers actions for each one of them. Action is a kind of trigger that will be triggered when all the predetermined thresholds are reached.

## How to configure? 

In workspace configuration, click on the section **Add Metric Action** and follow the steps: 

**1. Add action configuration**;  
**2. Type a nickname for your action;  
3. Type a description;  
4. Select a plugin to run the action.** 

![](../../.gitbook/assets/workspace_metricaction%20%281%29.gif)

{% hint style="info" %}
The available plugins are **circle deployment** and **circle undeployment**. Charles allows you to make your own plugin, the way you want and to fulfill your application's purpose, for example, an action that sends an email to warn your circle status.
{% endhint %}

Inside your circle, you register the [**metrics group**](../../reference/metrics/metrics-group.md), which is responsible to create metrics that you want to track and you can also add action to this group. When you get to the threshold, the action will be triggered and what is described in your plugin will happen, like for example, make deployment of one circle in another circle. 

For more information about **Action**, check out the [**Reference section**](../../reference/metrics/action.md). 

