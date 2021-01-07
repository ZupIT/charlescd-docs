# Installing via Single File

{% hint style="info" %}
Before proceeding, make sure that all the [**requirements**](./#requirements) are properly installed.
{% endhint %}

This installation is recommended for those who never used Charles before and just want the **first contact in a testing environment**, without looking for scalability or security.

In this case, you will have to:

* Use a **yaml** file with all the [**components**](https://docs.charlescd.io/get-started/installing-charles#components);
* Use a **Load Balancer** previously configured.

### How to install? 

{% hint style="danger" %}
This installation uses standard passwords that can be found in our repository. To change them, you have to choose [**helm installation**](installing-via-helm.md) where you can make the password change. 
{% endhint %}

To create this structure, you have to execute the files in a configured cluster, such as minikube, microk8s, GKE, EKS, etc. The steps to be executed are:

```text
kubectl create namespace charles

kubectl apply -n charles -f https://raw.githubusercontent.com/ZupIT/charlescd/master/install/helm-chart/single-file.yaml
```

At the end of the process, you will have inside of the namespace `charles` all the modules of the project and its dependencies installed in a simpler way. Here you will find the[ **files in the repository**](https://raw.githubusercontent.com/ZupIT/charlescd/master/install/helm-chart/single-file.yaml). 

{% hint style="info" %}
If you want to use this installation in a productive or development environment you will probably expose the application using a DNS.

After doing this, clone the single-file.yaml and change all occurrences from http://charles.info.example to &lt;your-dns&gt;, then execute the install command again.

 `kubectl install -f <single-file-path> -n charles`
{% endhint %}

{% hint style="danger" %}
The purpose of this installation is only for tests. Using this for production environment is not recommended due to lack o backup, high availability, etc.
{% endhint %}

