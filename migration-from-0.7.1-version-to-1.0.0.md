---
description: >-
  In this section, you will find how to migrate from Charles 0.7.1 version to
  the 1.0.0 one.
---

# Migration from 0.7.1 version to 1.0.0

The new CharlesCD's 1.0.0 version brought more reliability in deployments with [**Operator**](), however, Helm finds some difficulties to add the necessary elements to Operator in an existing installation, like  **Custom Resources Definition** \(CRDs\). To change that, you need to follow some steps for a more functional update. See below: 

### **Step 1: Uninstall CharlesCD \(installation with Helm\):**

{% hint style="warning" %}
This step **removes everything**, including the database. If your database is **external** or if it is **inside Kubernetes**, your data won't be erased, because the volumes persist even after the Helm is uninstalled. 
{% endhint %}

To uninstall, run the following command: 

```text
helm uninstall -n <NAMESPACE_CHARLESCD> <NOME_DA_INSTALAÇÃO>
```

### **Step 2: Install CharlesCD using Helm:**

Now, install CharlesCD via _Helm._ Use your _Values_ file of the original installation validating for each manifest if there were new values addition.

{% hint style="info" %}
You need to validate making a diff of your installation file and the available in CharlesCD's repository. 
{% endhint %}

To install, run the command below: 

```text

helm install -n <NAMESPACE_CHARLESCD> <NOME_DA_INSTALAÇÃO> ./install/helm-chart/ -f ./install/helm-chart/meu-values.yaml

```

### **Step 3: Update the deployment configuration for each Workspace:**

After installing, you can access CharlesCD with your admin user. It is necessary to do a new deployment configuration for your workspaces. See how to this configuration in [**Deployment Configuration**]().  

###  **Step 4: Override the release in each active circle:**

Now, you need to override the release with the latest release where each active circle deployment has been made. See how to perform version overrides in [**'How to create Charles release?**](reference/releases.md#how-to-create-releases-with-charles)'.

{% hint style="info" %}
Don't worry about the override release. CharlesCD will recognize all manifest it needs to take care of. 
{% endhint %}

### **Step 5: Remove all the manifests where CharlesCD made the deployments before the 1.0.0 version:** 

You need to remove all manifests CharlesCD made deployment before the 1.0.0 version. When deleting these old manifests, the Operator can recreate them, and then your application will work in less than 30 seconds. The unavailability is on average 5 seconds.  

Follow the steps to remove:

1. You need to know all existing manifests in the namespace of your workspace that can be deleted. For that, run the following command by workspace: 

```text
kubectl api-resources --verbs=list --namespaced -o name | xargs -n 1 kubectl get --show-kind --ignore-not-found  -n <NAMESPACE_WORKSPACE> -o go-template --template '{{range .items}}{{.kind}}/{{.metadata.name}} {{.metadata.creationTimestamp}}{{"\n"}}{{end}}' 2>&1 | grep -i -v "Warn" | grep -i -v "Deprecat" | awk '$2 <= "'$(date -d '1 hour ago' -Ins --utc | sed 's/+0000/Z/')'" { print $1 }' | grep -i -v "ConfigMap/istio" | grep -i -v "ConfigMap/kube" | grep -i -v "Secret/default" | grep -i -v "ServiceAccount/default"

```

This command shows a manifest list related to your workspaces older than 1 hour, which means everything that has not been deployed in the namespace of your Kubernetes. See the example below: 

![](.gitbook/assets/image%20%2822%29.png)

2. If there aren't applied manifests outside CharlesCD, a **Nginx** can work as an ingress for your application, for example. Add the command: 

```text
 | grep -i -v "PALAVRA_CHAVE"
```

See how the Nginx example works: 

```text
 
kubectl api-resources --verbs=list --namespaced -o name | xargs -n 1 kubectl get --show-kind --ignore-not-found  -n charlesapp -o go-template --template '{{range .items}}{{.kind}}/{{.metadata.name}} {{.metadata.creationTimestamp}}{{"\n"}}{{end}}' 2>&1 | grep -i -v "Warn" | grep -i -v "Deprecat" | awk '$2 <= "'$(date -d '1 hour ago' -Ins --utc | sed 's/+0000/Z/')'" { print $1 }' | grep -i -v "ConfigMap/istio" | grep -i -v "ConfigMap/kube" | grep -i -v "Secret/default" | grep -i -v "ServiceAccount/default" | grep -i -v "nginx"

```

This command removes the manifests list and after you will the complete list to remove the cluster.

3. When you have this manifests list, delete all the manifests and the rest will be executed by the Operator. Run the command below: 

```text
kubectl delete -n <namespace> <manifesto>
```

{% hint style="success" %}
Done! The update process is complete :\) 
{% endhint %}

