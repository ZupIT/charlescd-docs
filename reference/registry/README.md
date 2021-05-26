---
description: 'In this section, you will find information about the registry.'
---

# Registry

## Why do you have to configure a Registry? 

One of the main points for Charles to work is to know where are the images of your applications. To do this, you are expected to store them in a docker registry and grant access to it. 

Once you make this configuration, Charles is able to read your registry and do some actions like: 

* Validates if the tag you are informing when deploying in a circle is valid; 
* During the generation of a release through the board, `villager` uses this permission to search for the tag in the registry and ensure that it has been sent and can be used.

Charles is already integrated with some docker registries, choose one and add the information:

{% page-ref page="azure-container-registry.md" %}

{% page-ref page="docker-hub.md" %}

{% page-ref page="ecr.md" %}

{% page-ref page="gcr.md" %}

