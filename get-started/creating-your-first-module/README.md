# Creating your first module

After creating and configuring your workspace, it is necessary to add the modules.

{% hint style="info" %}
A **module** is your application stored in a [**Git repository previously registered**](https://docs.charlescd.io/get-started/defining-a-workspace).
{% endhint %}

To add it, access the **Modules** menu on your workspace and set the following properties:

* **Name**: This field should be the junction of the organization and the module names, as it is in your git. For example: `ZupIt/charlescd`.
* **Git URL**: Your repository's URL. For example: [https://github.com/ZupIT/charlescd](https://github.com/ZupIT/charlescd).
* **Helm repository URL:**  your repository where the helm template of your application is in. 

{% hint style="info" %}
For more information about Helm Repository, [**access here**](how-to-configure-chart-template.md). 
{% endhint %}

If you have a lot of applications on your repository, register them with the components and add the following:

* **Name**: application's name, the same on your repository;
* **Metrics**: latency \(ms\) and HTTP error \(%\). In both cases, you must add a risk value that you want to receive an alert if your component reaches the rate. 

In both cases, you need to inform the minimum risk value you'd like to be alerted.

![Creating a module screen](../../.gitbook/assets/criac-a-o-de-modulo%20%282%29%20%281%29.png)

## Components

{% hint style="info" %}
Components are abstractions of the applications. If in your repository there are many applications, every component will match one of them.
{% endhint %}

