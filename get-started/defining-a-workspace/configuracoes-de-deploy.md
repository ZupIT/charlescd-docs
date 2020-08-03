# Deploy environment

It is necessary to register cluster [**Kubernetes**](https://kubernetes.io) credentials to configure your workspace. These are specific configuration to each Continuous Deployment \(CD\) tool that are integrated with Charles, at the moment it is [**Spinnaker**](https://www.spinnaker.io/) and Octopipe.

{% hint style="info" %}
**Octopipe** was developed by Charles' team. It is light, low cost and it is able to make cluster Kubernetes deploys.
{% endhint %}

See below the example on how to perform your deploy using Octopipe in the same installation cluster:

### How to make your deploy?

Segue abaixo o exemplo de como realizar seu deploy utilizando o _Octopipe_ no mesmo cluster de instalação:

1. Click on **Add CD Configuration**;
2. Select the option **Octopipe.**

After these steps, fill out the next fields:

1. **Name:** configuration name that will be created; 
2. **Namespace:** Define the namespace that will be used on Kubernetes cluster deploys; 
3. **Git provider**: Define the git provider \(**GitHub** or **GitLab**\);
4. **Git token:**  Insert the authentication token for your git repository. This will be used to get **Helm** templates.  
5. Select the **Default** option.

After finish your configuration, you can associate it with a module later. For more information, check the [**CD Configuration**](https://docs.charlescd.io/reference/cd-configuration) page. 

