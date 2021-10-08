---
description: 'In this section, you will find how to configure chart template.'
---

# How to configure Chart template

## What is Helm? 

Helm Charts is a package manager that allows you to define, install and update Kubernetes applications, regardless of the complexity.

### Chart template on CharlesCD's context

 [**Chart Template**](https://helm.sh/docs/chart_template_guide/getting_started/) ****is used like a collection file related to the Kubernetes configuration.

The charts must follow the [**Helm pattern**](https://helm.sh/docs/topics/charts/) ****and they need to be contained in a folder with the registered component on Charles. You don't need to run any command to bundle the chart, Charles downloads and it finishes automatically.   
  
See the below an example of a repository containing the component's chart  **http-https-echo** on GitHub:

![](https://lh5.googleusercontent.com/Rt7_Lw1DbK152QKt3brsCYyzF0DAQ4wuoWsdCVyUaZjf9Hlh64EaK7YnHjF16W_xo2BQzlUJyUeUsooPzqwmMIKF7ttUXRej3eM56uWu6WH4QNCiByixeV4zEdHLwEGRq7NCruhH)

The deployment module Butler uses helm charts to make your application available in the cluster.  These charts must be available in a Github or Gitlab repository and accessible through a token, previously registered in the deployment configuration. The URL is provided along with the module registration. 

{% hint style="info" %}
If you haven't configured your module yet, [**access the tutorial on how to create one**](./). You have to register the URL in this module too. 
{% endhint %}

### **Templates**

The only requirements for templates to work with Charles are **labels component** and **tag** to be present in the Deployment resource manifests. 

{% hint style="warning" %}
It is not necessary to insert the values in the _**values**_ files of your chart, Charles will provide them. 
{% endhint %}

See the example below: 

```text
component: {{ .Values.component }}
tag: {{ .Values.tag }}
```

Butler internally stores the compiled charts in entities that represent each deployment request. Thereby, Charles can perform more efficient rollbacks.

## How to configure the chart template?

Follow the next steps to try out our sample app.

### **Step 1: Create a chart template directory**

To start, you need to save your templates in any git repository you want. When you create a new chart template, you must give the directory the same name as the component it refers to. 

The structure below has the necessary templates to deploy a module that contains a component called "circles-sample", it is available here. 

The image below shows how your directory must look like: 

![ Chart template directory of circle-sample](../../.gitbook/assets/screen-shot-2020-08-13-at-09.16.04.png)

### Step 2: configure the directory items 

After you have created the directory, now you have to configure it. See below which files are necessary to configure: 

* **templates/** : it has the models.

  * **deployment.yaml:** describes the [**deployment**](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) ****structure.  
  * **service.yaml:** describes the [**service**](https://kubernetes.io/docs/concepts/services-networking/service/) ****structure. ****

* The **Chart.yaml** file contains the descriptions as version, name, description. It is necessary to define the version as "darwin".
* The **circles-sample.yaml** file has the values that will be used in the templates. 

This information Charles needs to have on the templates. It is important to remember that you can customize these templates the way you want them. 

{% hint style="info" %}
After you have configured your directory according to the structure above, go to the "circles-samples" folder and run the command **"`helm package .`"**. 

At the end of this command, you will have a **tgz** file with the circles-samples-darwin name. Our CD tool looks for this **tgz** to run the template.
{% endhint %}

