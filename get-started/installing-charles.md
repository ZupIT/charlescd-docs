# Installing Charles

## Requirements

To install Charles it will be necessary an environment with the following requisites: 

* [**Kubernetes**](https://kubernetes.io/docs/setup/).
* \*\*\*\*[**Istio**](https://istio.io/archive/) ****\(version&lt;= 1.4  and [**enabled sidecar injection**](https://istio.io/latest/docs/setup/additional-setup/sidecar-injection/#automatic-sidecar-injection) ****on the deploy namespace of your application\).
* \*\*\*\*[**Prometheus**](https://prometheus.io/docs/prometheus/latest/getting_started/)**,** in case you want to use ****[**metrics**](../reference/metrics/)**.** 

### Resources 

The minimum resources considering only the installation of Charles are: 

* **Microk8s**: 2GB of RAM; 
* **Minikube**: 4GB of RAM. 
* **Cluster**: 2GB of RAM.

## Introduction

The installing process was created considering some use cases, and each of them have its own specific tutorial.  But, before this, check out below which components and platforms Charles supports.

{% hint style="info" %}
If you need to install CharlesCD with more customization, we suggest to check the[ **installation with helms charts section**.](https://docs.charlescd.io/get-started/installing-charles#case-2-installation-with-helm-charts)
{% endhint %}

### Components

CharlesCD's installation considers these components:

1. **Charles' architecture** specific modules; 
2. **Keycloak**, used for authentication and authorization;
3. A **PostgreSQL database** for backend modules \(`moove`, `circle-matcher`, `butler` and `villager`\) and Keycloak;
4. A **Redis**, to be used by `charlescd-villager`
5. **Octopipe** is in the standard Charles installation as a CD option. In addition, a **MongoDB** is installed for its use. However, the usage of Octopipe is optional, it is possible to disable it at installation.

### Continuous Delivery Platform

At this moment, Charles can support two Continuous Delivery \(CD\) platforms:

* **Spinnaker:** if you have your spinnaker already configured, it can be reused.
* **Octopipe:** a native platform created by our team to make installation easier, without previous configurations. 

{% hint style="info" %}
If you want more information about how to configure Spinnaker or Octopipe, check the [**CD Configuration**](https://docs.charlescd.io/reference/cd-configuration) section.
{% endhint %}

## Main installation cases

At the first access, **regardless of the installation method**, the default admin user is **charlesadmin@admin** and the password is **charlesadmin.**

### Case \#1: Quick installation

This installation is recommended for those who never used Charles before and just want a **first contact in a testing environment**, without looking for scalability or security.

In this case, you will have to:

* Use a **yaml** file with all the [**components**](https://docs.charlescd.io/get-started/installing-charles#components);
* Use a **Load Balancer** previously configured.

To create this structure, you have to execute the files in a configured cluster, such as minikube, microk8s, GKE, EKS, etc. The steps to be executed are:

```text
kubectl create namespace charles

kubectl apply -n charles -f https://raw.githubusercontent.com/ZupIT/charlescd/master/install/helm-chart/single-file.yaml
```

At the end of the process, you will have inside of the namespace `charles` all the modules of the project and its dependencies installed in a simpler way. Here you will find the[ **files in the repository**](https://raw.githubusercontent.com/ZupIT/charlescd/master/install/helm-chart/single-file.yaml).   


### **How to access the application?**

There are three ways to access the application: through **Minikube,** **Microk8s** or **Cloud provider.**  See below how the configuration works on each one of them:

### **Minikube**

On the minikube, the **load balancer** does not automatically create an **external IP,** to make this possible, follow the steps: 

**Step 1**: Just run the command below:

```text
minikube tunnel
// enter your root password, then open another terminal tab and run:
kubectl get svc -n charles
// now the nginx IP external appears
```

**Step 2:** Now that you have the **external ip,** **replace the ip-external-charles** and add this line on your host file:

```text
<IP-EXTERNAL-CHARLES>       charles.info.example
```

{% hint style="info" %}
For more information on **how to change the host file,** [**access here.** ](https://www.howtogeek.com/howto/27350/beginner-geek-how-to-edit-your-hosts-file/)\*\*\*\*
{% endhint %}

**Step 3:** In your browser type **http://charles.info.example** and the entire application is available.

### Microk8s

Microk8s is available for Microsoft Windows, Apple MacOS and Linux platforms. 

{% hint style="info" %}
For more information on how to install Microk8s visit the [**project's website**](https://microk8s.io/)**.**
{% endhint %}

Once Microk8s is installed, you have to enable the following add-ons:

* **DNS:** discovery of services within the cluster; 
* **Storage:** creating volumes and persistence of PODs; 
* **MetalLB:** access to the services exposed by kubernetes - For this addon, you will have to choose a range of IPs where Load Balancer will assign for the exposure of its services.

Follow the next steps to enable Microk8s on Charles: 

**Step 1:** prepare Microk8s to receive the CharlesCD;

```text
microk8s enable dns storage metallb
Enabling DNS
Applying manifest
serviceaccount/coredns created
configmap/coredns created
deployment.apps/coredns created
service/kube-dns created
clusterrole.rbac.authorization.k8s.io/coredns created
clusterrolebinding.rbac.authorization.k8s.io/coredns created
Restarting kubelet
DNS is enabled
Enabling default storage class
deployment.apps/hostpath-provisioner created
storageclass.storage.k8s.io/microk8s-hostpath created
serviceaccount/microk8s-hostpath created
clusterrole.rbac.authorization.k8s.io/microk8s-hostpath created
clusterrolebinding.rbac.authorization.k8s.io/microk8s-hostpath created
Storage will be available soon
Enabling MetalLB
Enter each IP address range delimited by comma 
(e.g. '10.64.140.43-10.64.140.49,192.168.0.105-192.168.0.111'):
// In this step, you can choose a range or use the suggested one, we will use
// 10.64.140.43-10.64.140.49
10.64.140.43-10.64.140.49
Applying registry manifest
namespace/metallb-system created
podsecuritypolicy.policy/speaker unchanged
serviceaccount/controller created
serviceaccount/speaker created
clusterrole.rbac.authorization.k8s.io/metallb-system:controller unchanged
clusterrole.rbac.authorization.k8s.io/metallb-system:speaker unchanged
role.rbac.authorization.k8s.io/config-watcher created
clusterrolebinding.rbac.authorization.k8s.io/metallb-system:controller unchanged
clusterrolebinding.rbac.authorization.k8s.io/metallb-system:speaker unchanged
rolebinding.rbac.authorization.k8s.io/config-watcher created
daemonset.apps/speaker created
deployment.apps/controller created
configmap/config created
MetalLB is enabled

// With microk8s configured, we can then install Charles using
// the single-file
microk8s.kubectl create namespace charles
​​microk8s.kubectl apply -n charles -f https://raw.githubusercontent.com/ZupIT/charlescd/master/install/helm-chart/single-file.yaml

// now the nginx IP external appears
microk8s.kubectl get svc -n charles

```

**Passo 2:** now that you have the **external ip,** **replace the ip-external-charles,** add this line on your OS host file:

```text
<IP-EXTERNAL-CHARLES>       charles.info.example
```

{% hint style="info" %}
For more information on **how to change the host file,** [**access here.** ](https://www.howtogeek.com/howto/27350/beginner-geek-how-to-edit-your-hosts-file/)\*\*\*\*
{% endhint %}

**Step 3:** type in your browser **http://charles.info.example** and the entire application will be  available.

### **Cloud Provider \(AWS, GCP, AZURE\)**

On Cloud Provider, if you install on a managed kubernetes, **the external ip for the nginx load balancer is created automatically**, 

When all the components are ready follow the next steps:

**Step 1:** Just take the external IP with the command below and add it to your hosts file.

```text
kubectl get svc -n charles
// get external IP value
```

**Step 2:**  Add the line below in you [OS host file](https://www.howtogeek.com/howto/27350/beginner-geek-how-to-edit-your-hosts-file/), if you want to access the browser in your device. 

```text
<IP-EXTERNAL-CHARLES>       charles.info.example
```

{% hint style="info" %}
If you want to use this installation in a productive or development environment you will probably expose the application using a DNS.

After doing this, clone the single-file.yaml and change all occurrences from http://charles.info.example to &lt;your-dns&gt;, then execute the install command again.

 `kubectl install -f <single-file-path> -n charles`
{% endhint %}

{% hint style="danger" %}
The purpose of this installation is only for tests. Using this for production environment is not recommended due to lack o backup, high availability, etc.
{% endhint %}

### Case \#2: Installation with helm charts

This installation is recommended for those who already has an infrastructure to deal with a more complex environment or who has some limitations of security/scalability, which demands a **more complete install customization** of CharlesCD.  

### Requisites 

To run the process, you must have the following programs:

* \*\*\*\*[**Kubectl**](https://kubernetes.io/docs/tasks/tools/install-kubectl/)\*\*\*\*
* [**Helm** ](https://helm.sh/docs/intro/install/)\*\*\*\*

### How does it works?

This installation is recommended if you want a specific customization. To make this happen, there is a helm template with all the available fields to be altered, including the database and consumed resources. You will find the documentation with the[ **editable fields here**](https://github.com/ZupIT/charlescd/tree/master/install/helm-chart).

To complete the installation with helm charts, just run the command below after you customized the fields: 

```text
// customize everything you need in the file values.yaml before you execute the following command
helm install charlescd <repo-folder> -n <namespace>
```

{% hint style="warning" %}
It's important to remember that, in case of no customization at all, the final result is the same as in case \#1 in which, for standard, we install the PostgreSQL, Redis, Keycloak and Octopipe. 

So, you must not forget to customize the fields in case you want something manageable. 
{% endhint %}

## Authenticating the cluster with your registry 

### Why do you need to authenticate?

Authentication is required if you use a private registry. This way, the cluster will be able to communicate with your registry to pull the images.

### How do you authenticate?

Kubernetes cluster uses a type of docker-registry **Secret** to authenticate the registry container. You have to generate it. 

{% hint style="info" %}
 For more information on how to generate the Secret that will be applied in your cluster, [**access Kubernetes documentation**](https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/). 
{% endhint %}

Once you generate the secret, it will look like:

```text
apiVersion: v1
data:
  .dockerconfigjson: <<your value>>
kind: Secret
metadata:
  name: <<registry-name>>
type: kubernetes.io/dockerconfigjson
```

After this, you'll need to apply this information in the namespace where your applications will be deployed by Charles:

```text
kubectl -n your-namespace apply secret-registry.yaml
```

After completing these steps, your cluster will be able to maintain communication with the registry.

