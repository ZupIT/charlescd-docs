# First Deploy

{% hint style="info" %}
After you have created your first [**module**](creating-your-first-module/) and registered your [**cluster crendentials**,](defining-a-workspace/deploy-environment.md) you have finished all the steps needed to make your first deployment. Now, it is necessary to create a [**release** ](../reference/releases.md)and provide it on the configured cluster.
{% endhint %}

### How to make the first deployment? 

On Charles you have to use container images already available in your configured [**registry**](../reference/registry/) to create a release.

To make your first deployment, follow the steps below: 

1. Go to **Circles** area;
2. Select a [**circle**](../reference/circles.md). If you haven't created one yet, there is a **default circle** option that makes your first deploy possible; 
3. Change the active circle filter to **inactive**;
4. Select the "**Insert a release**" option;
5. After that, select "**Create a release**" and fill the fields: 
   * **'Release name':** choose a name for your release;
   * **'Select a module';**
   * **Select a component';**
   * **Version name**': type the name of your tag \(it is necessary to be the same one shown in your Registry\). 

5 . Click on '**Deploy**' button and wait for a status on the green card. When processing, you will see "**deploying**", but at the end, it will show up as "**deployed**".

{% hint style="success" %}
After the process above, your release is ready to deploy. 
{% endhint %}

### Open Sea deploy

The [**open sea**](../key-concepts.md#open-sea) deployment is where you send your application to the registered segmentation on Charles.

Now, follow the next steps to the [**Open Sea**](https://docs.charlescd.io/key-concepts) deploy:

1. On Charles homepage, click on **Circles**; 
2. Click on the Default circle \(it represents the open sea\) 
3. Click on **Override release** in upper right corner; 
4. Click on **Search for ready releases**;
5. Type the release name created above \(or use it again with a new version\) and click on **Deploy**.

Finally, Charles will provide the created release on a cluster in the Open Sea. The deploy status will be shown and updated along the process.

![](../.gitbook/assets/first-deploy%20%281%29.gif)

