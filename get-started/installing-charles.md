# Installing Charles

## Requisites 

To install Charles it will be necessary an environment with the following requisites: 

* [**Kubernetes**](https://kubernetes.io/docs/setup/).
* \*\*\*\*[**Istio**](https://istio.io/archive/) ****\(version&lt;= 1.4\).
* \*\*\*\*[**Prometheus**](https://prometheus.io/docs/prometheus/latest/getting_started/)**,** in case you want to use ****[**metrics**](../reference/metrics/)**.** 

## Introduction

The installing process was created considering some use cases, and each of them have its own specific tutorial.  But, before this, check out below which components and platforms Charles supports.

{% hint style="info" %}
If you need to install CharlesCD with more customization, we suggest to check the[ **installation with helms charts section**.](https://docs.charlescd.io/get-started/installing-charles#case-2-installation-with-helm-charts)
{% endhint %}

### Components

The CharlesCD installation considers these components:

1. **Charles' architecture** specific modules; 
2. **Keycloak**, used for product authentication and authorization;
3. A **PostgreSQL database** for backend modules \( `charles-application`, `charles-circle-matcher`, `deploy` and `villager`\) and Keycloak;
4. A **Redis**, to be used by `charlescd-villager`

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

To create this structure, you have to execute the files in a configured cluster, such as minikube, GKE, EKS, etc. The steps to be executed are:

```text
kubectl create namespace charles

kubectl apply -n charles -f https://raw.githubusercontent.com/ZupIT/charlescd/master/install/helm-chart/single-file.yaml
```

At the end of the process, you will have inside of the namespace `charles` all the modules of the project and its dependencies installed in a simpler way. Here you will find the[ **files in the repository**](https://raw.githubusercontent.com/ZupIT/charlescd/master/install/helm-chart/single-file.yaml).   


### **How to access the application?**

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

### **Cloud Provider \(AWS, GCP, AZURE\)**

If you install on a managed kubernetes, **the external ip for the nginx load balancer is created automatically**, so when all the components are ready follow the steps:

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

