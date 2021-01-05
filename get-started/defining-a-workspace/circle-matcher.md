# Circle Matcher

## Why do you have to configure?

When [**creating a workspace**](./), you have to inform Charles to which Circle Matcher that current workspace will point to. It is possible that there is a Circle Matcher for each environment, since Charles can handle, at the same time, different environments in multiple workspaces.

Circle Matcher is a independent module, despite that, it is possible to install it in any area you want inside its architecture, for example, a public cluster.

This configuration is necessary, so you are able to perform operations in Charles, like creating and editing segments in a circle.

{% hint style="info" %}
It is important to remember, on Charle's context, the Circle Matcher module receives most of the environment's request, because it is the application that identifies the user based on the rules that you have configured while managing a circle.
{% endhint %}

 If you want to know more about **Circle Matcher**, see the [**References section**](../../reference/circle-matcher.md). 

## How must be configured 

#### Option 1: Configure Circle Matcher in a separate architecture

You have to configure the public DNS that points to your desired Circle Matcher.

> Example: **http://charles.info.example/charlescd-circle-matcher**.



#### Option 2: Configure Circle Matcher in the same Charles' namespace  

If you want to use Circle Matcher in the same namespace that Charles is installed, you can use the same DNS reference.

The difference is in terms of performance, it is recommended to use Kubernetes service name.

> Example: **http://charlescd-circle-matcher:8080**.

## Next steps

On this section, you saw how to create your Circle Matcher. To continue your workspace configuration, Charles offers metrics that need to be configured. 

👉 Go to [**Setting up your metrics** ](../../reference/metrics/setting-up-your-metrics.md)and find out how Charles uses metrics. 

