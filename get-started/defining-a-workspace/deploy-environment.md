---
description: 'In this section, you will find information about the deployment environment.'
---

# Deploy environment

The deployment module monitors and applies the cluster resources. To do that, it uses the [**Operator**](https://kubernetes.io/docs/concepts/extend-kubernetes/operator) pattern that performs reconciliation cycles to make sure that the cluster will always be in the state you need. Kubernetes' logs will also be collected in real-time.

### **Configuring the workspace**

It is necessary to register cluster [**Kubernetes**](https://kubernetes.io) credentials to configure your workspace. These are specific configurations to each Continuous Deployment \(CD\) tool that is integrated with Charles. At the moment, Charles has a native deploy or you can integrate with [**Spinnaker**](https://www.spinnaker.io/).

## **Deployment configuration**

Charles has an architecture that adapts to different Kubernetes installations. The only requirement is that your deployment module is installed on the destination cluster with an accessible URL. The deployment configuration indicates what URL is and which Git credentials will be used to search the helm charts. Without this configuration, Charles won't be able to perform deploys. 

### **How can you register the configuration?**

Follow the steps below: 

1. In **Workspaces,** at the left menu, select your workspace and then click on **Settings ;**
2. Click on **Credentials;**
3. Click on **Add Deployment Configuration;**

Fill in these fields:

1. **Butler URL:**  Butler's deploy module URL. If this is in the same Charles' installation cluster, use your FQDN \(Fully Qualified Domain Name\). Example: **http://charlescd-butler.butler-namespace.svc.cluster.local:3000.**
2. **Namespace:** Define the namespace where the resources will be available in the cluster. You have to create your namespace, once Charles does not do it;
3. **Git provider:** defines the git provider you will use ****\(**GitHub or GitLab**\);
4. **Git token:** Insert an **authentication token** that has access to the git repository where your [**helm templates**](../creating-your-first-module/) ****are stored \(they will be used during the deployment of your application\). If your Git Provider is **GitHub**, "_repo_" permission is required**.** Otherwise, configure the accesses in **GitLab:** "_API_" and "_read\_repository_".

{% hint style="warning" %}
In the authentication token field to avoid the dependency of a specific user, use[ **Machine Account**](https://docs.github.com/en/developers/overview/managing-deploy-keys#machine-users)**.**
{% endhint %}

After finishing your configuration, you can associate it with a module later. For more information, check the [**CD Configuration**](https://docs.charlescd.io/reference/cd-configuration) page. 

