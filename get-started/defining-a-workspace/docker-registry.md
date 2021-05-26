---
description: 'In this section, you will find information about docker registry.'
---

# Docker registry

{% hint style="warning" %}
This is mandatory information.
{% endhint %}

One of the steps to configure your workspace is to inform Charles which docker registry you store your application's images. This access is important because CharlesCD can watch newly generated images and list the ones already saved in your registry to deploy them in circles.

Charles is already integrated with some docker registries, choose one and add the information:

{% page-ref page="../../reference/registry/azure-container-registry.md" %}

{% page-ref page="../../reference/registry/docker-hub.md" %}

{% page-ref page="../../reference/registry/ecr.md" %}

{% page-ref page="../../reference/registry/gcr.md" %}



