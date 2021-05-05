# Deploy environment

It is necessary to register cluster [**Kubernetes**](https://kubernetes.io) credentials to configure your workspace. These are specific configurations to each Continuous Deployment \(CD\) tool that is integrated with Charles. At the moment, Charles has a native deploy or you can integrate with [**Spinnaker**](https://www.spinnaker.io/).

{% hint style="info" %}
Charles has a module called **Octopipe** that is a low-cost and lighter way to make cluster Kubernetes deploys.
{% endhint %}

### How to make your deployment?

See below the example on how to perform your deploy using **CharlesCD** in the same installation cluster:

1. Click on **Add CD Configuration**;
2. Select the option **CharlesCD.**

After these steps, fill out the next fields:

1. **Name:** configuration name that will be created; 
2. **Namespace:** Define the namespace that will be used on Kubernetes cluster deploys; 
3. **Git provider**: Define the git provider \(**GitHub** or **GitLab**\);
4. **Git token:** insert an authentication token that has access to the git repository where your [**Helm templates**](../creating-your-first-module/how-to-configure-chart-template.md) ****are stored \(they will be used during the deployment of your [**application**](../creating-your-first-module/)\). If your Git Provider is **GitHub**, "_repo_" permission is required. Otherwise, configure the accesses in **GitLab**:"API " and "_read\_repository_".
5. Select the **Default** option.

After finishing your configuration, you can associate it with a module later. For more information, check the [**CD Configuration**](https://docs.charlescd.io/reference/cd-configuration) page. 

