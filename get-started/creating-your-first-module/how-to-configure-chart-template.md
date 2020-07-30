# How to configure Chart template

## What is Helm? 

Helm Charts  is package manager that allows you to define, install and update Kubernetes applications, regardless the complexity.

On Charles context,  [**Chart Template**](https://helm.sh/docs/chart_template_guide/getting_started/) ****is used like a file collection related to the Kubernetes configuration.

{% hint style="info" %}
If you haven't configured your module yet, [**access here**](./)**.** It is important to remember, you have to register the URL in this module. 
{% endhint %}

## How to configure the chart template?

Follow the next steps to try out our sample app.

### **Step 1: create a chart template directory**

To start, you need to save your templates in any git repository you want. When you create a new chart template, you must give the directory the same name as the component as it refers to. 

The structure below has the necessary templates to deploy a module that contains a component called "circles-sample", it is available here. 

The image below shows how you directory must look like: 

![ Chart template directory of circle-sample](../../.gitbook/assets/screen-shot-2020-07-24-at-16.17.05.png)

### Step 2: configure the directory items 

After you have created the directory, now you have to configure it. See below which files are necessary to configure: 

* **templates/** : it has the models.

  * **deployment.yaml:** describes the [**deployment**](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) ****structure.  
  * **service.yaml:** describes the [**service**](https://kubernetes.io/docs/concepts/services-networking/service/) ****structure. ****

* The **Chart.yaml** file contains the descriptions as version, name, description. It is necessary to define the version as "darwin".
* The **value.yaml** file has the values that it will be used in the templates. 

This information Charles needs to have on the templates. It is important to remember that you can customize these templates the way you want it. 

{% hint style="info" %}
After you have configured your directory according to the structure abpve, go to the "circles-samples" folder and run the command **"`helm package .`"**. 

In the end of this command, you will have a **tgz** file with the circles-samples-darwin name. Our CD tool looks for this **tgz** to run the template.
{% endhint %}

