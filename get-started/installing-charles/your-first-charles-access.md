# Your first Charles' access

{% hint style="info" %}
At the first access, **regardless of the installation method**, the default admin user is **charlesadmin@admin** and the password is **charlesadmin.**

It is important that, after your first login, you change this password.
{% endhint %}

There are three ways to access the application: 

* **Minikube;**
* **Microk8s;**
* **Cloud provider.**  

See below how the configuration works on each one of them:

## **1. Minikube**

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

## 2. Microk8s

Microk8s is available for Microsoft Windows, Apple MacOS, and Linux platforms. 

{% hint style="info" %}
For more information on how to install Microk8s visit the [**project's website**](https://microk8s.io/)**.**
{% endhint %}

Once Microk8s is installed, you have to enable the following add-ons:

* **DNS:** discovery of services within the cluster; 
* **Storage:** creating volumes and persistence of PODs; 
* **MetalLB:** access to the services exposed by Kubernetes - For this addon, you will have to choose a range of IPs where Load Balancer will assign for the exposure of its services.

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

**Step 3:** type in your browser **http://charles.info.example** and the entire application will be available.

## **3. Cloud Provider \(AWS, GCP, AZURE\)**

On Cloud Provider, if you install on a managed Kubernetes, **the external ip for the nginx load balancer is created automatically**, 

When all the components are ready follow the next steps:

**Step 1:** Just take the external IP with the command below and add it to your hosts' file.

```text
kubectl get svc -n charles
// get external IP value
```

**Step 2:**  Add the line below in your [**OS host file**](https://www.howtogeek.com/howto/27350/beginner-geek-how-to-edit-your-hosts-file/), if you want to access the browser on your device. 

```text
<IP-EXTERNAL-CHARLES>       charles.info.example
```

{% hint style="info" %}
If you want to use this installation in a productive or development environment you will probably expose the application using a DNS. 

After doing this, clone the configurations \(it can be single-file or the helm files\) and change all occurrences from http://charles.info.example to your new DNS, then run the install command again.
{% endhint %}

{% hint style="danger" %}
The purpose of this installation is only for tests. Using this for the production environment is not recommended due to lack o backup, high availability, etc.
{% endhint %}

