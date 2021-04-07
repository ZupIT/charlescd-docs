---
description: 'In this section, you will find more information about releases on Charles.'
---

# Releases

Releases are application versions. It is different from other ways of deployment that releases generally go through lots of environments until they reach production. However, with CharlesCD it is possible that the same release will be published for different[ **circles**](circles.md).

## How to create releases with Charles?

### **Releases through existing images on configured Docker Registry**

In this option, it is necessary that the Docker images are already available on your [**configured registry** ](../get-started/defining-a-workspace/docker-registry.md)for the module. If this requirement is done, just click on the [**circles**](circles.md)**'** option on Charles menu and select the circle for a release deploy to be created.

If you are creating the circle at this moment, click on **Insert release** and then Create release. If the circle is already created, click on **Override release** and then **Create release**.

On the release creation screen, fill in the name and select one module and its component. On the field beside, all available images on that component will be listed on the registry. Select one and, if it's necessary, add more modules to the release, clicking on **Add module** and repeat the previous process. When all your modules are registered, click on **deploy**.

![Exemple of release creation by Registry images](../.gitbook/assets/releases-por-meio-de-imagens-existentes%20%281%29%20%281%29.gif)

After deploying this new release, it will be available to use in other circles, just look into the '**Search for existing releases'** option.

