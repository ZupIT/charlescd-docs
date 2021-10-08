---
description: 'In this section, you will find how to install Charles.'
---

# Installing Charles

## Components

CharlesCD's installation considers these **components**:

1. **Charles' architecture** specific modules; 
2. **Keycloak**, used for the project's authentication and authorization. However, if you already have an Identity Manager \(IDM\) and you want to use it, you have just to configure it during Charles' installation, [**check out how  to enable it in the** **IDM section**](../../reference/identity-manager.md);
3. A **PostgreSQL database** for backend modules \(`charlescd-moove`, `charlescd-butler` ,`charlescd-villager`, `charlescd-gate` e `charlescd-compass`\) and Keycloak;
4. A **Redis:**  To be used by `charlescd-circle-matcher`
5. A **RabbitMQ** for `charlescd-hermes`' use.
6. **Ingress:** It is used to expose the HTTP and HTTPS routes outside the cluster to services inside the cluster. When you install Charles, it already has a default ingress, however, if you want to use your own, see how to enable it in the [**Installing via Helm section**](../optional-configuration/configuring-your-ingress.md). 

## Requirements

To install Charles will be necessary an environment with the following requisites: 

* [**Kubernetes**](https://kubernetes.io/docs/setup/).
* \*\*\*\*[**Helm**](https://helm.sh/docs/intro/install/)\*\*\*\*
* \*\*\*\*[**Istio**](https://istio.io/archive/) ****\(version&gt;= 1.7  and [**enabled sidecar injection**](https://istio.io/latest/docs/setup/additional-setup/sidecar-injection/#automatic-sidecar-injection) ****on the deploy namespace of your application\).
* \*\*\*\*[**Prometheus**](https://prometheus.io/docs/prometheus/latest/getting_started/)**,** in case you want to use ****[**metrics**](../../reference/metrics/)**.** 

## Resources 

The minimum resources considering only the installation of Charles are: 

* **Microk8s**: 2GB of RAM; 
* **Minikube**: 4GB of RAM. 
* **Cluster**: 2GB of RAM

## Next steps

In this section, you saw components, requirements, and resources to install Charles. To continue the installation, see: 

{% page-ref page="installing-via-helm.md" %}

{% page-ref page="your-first-charles-access.md" %}

