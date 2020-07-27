# How to configure Chart template

## What is Helm? 

Helm Charts  is package manager that helps define, install and update  the most complexes application on Kubernetes. Charles uses [**Chart Template**](https://helm.sh/docs/chart_template_guide/getting_started/), it is a file collection related to the Kubernetes configuration. 

## How to configure the chart template?

### **Chart template directory**

You can save your templates in any version tool you want. To create a new chart template you must give the directory the same name as the component as it refers to. The structure below has the necessary templates to deploy a module that contains a component called "circles-sample", it is available here. 

See the image below: 

![ Chart template directory of circle-sample](../../.gitbook/assets/screen-shot-2020-07-24-at-16.17.05.png)

### Directory items 

See below which files are necessary to configure your directory: 

* **templates/** : it has the models.

  * **deployment.yaml:** describes the [**deployment**](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) ****structure.  
  * **service.yaml:** describes the [**service**](https://kubernetes.io/docs/concepts/services-networking/service/) ****structure. ****

* The **Chart.yaml** file contains the descriptions as version, name, description. It is necessary to define the version as "darwin".
* The **value.yaml** file has the values that it will be used in the templates. 

This basic information is important, because Charles needs them to be in the templates. It is important to remember that you can customize these templates the way you want it. 

{% hint style="info" %}
After you have configured your directory according to the structure abpve, go to the "circles-samples" folder and run the command **"`helm package .`"**. In the end of this command, you will have a **tgz** file with the circles-samples-darwin name.
{% endhint %}

