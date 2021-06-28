---
description: Esta seção descreve como instalar o Charles no seu projeto.
---

# Instalando o Charles

## Componentes

A instalação do CharlesCD consiste nos seguintes **componentes**:

1. Módulos específicos da[ **arquitetura do Charles**](../../#arquitetura-do-sistema)**.**
2. **Keycloak**, usado para autenticação e autorização no projeto. Entretanto, caso você já tenha um Identity Manager \(IDM\) e queira utilizá-lo, basta fazer a sua configuração durante a instalação do Charles, ****[**veja como habilitá-lo na seção IDM**](../../referencia/identity-manager.md)**.**
3. Um **banco PostgreSQL** que servirá os módulos de backend \(`charlescd-moove`

   , `charlescd-butler`, `charlescd-villager`, `charlescd-gate, charlescd-hermes`

    e `charlescd-compass`\) e o Keycloak.

4. Um **Redis** para uso do `charlescd-circle-matcher`
5. Um módulo chamado **`octopipe`** está na instalação padrão do Charles. É uma plataforma nativa criada pelo time para que a instalação seja mais fácil e sem configurações prévias. Entretanto, é opcional e você pode desabilitá-la.
6. Um **RabbitMQ** para uso do `charlescd-hermes.`
7. **Ingress,** usado para expor as rotas HTTP e HTTPs de fora do cluster para serviços dentro do cluster. Quando você instala o Charles, ele já possui uma ingress padrão, no entanto se você quiser usar a sua própria, **veja os passos para habilitá-la na** [**seção Instalando via Helm**](../configuracoes-opcionais/configurando-sua-ingress.md)**.** 

##  Pré-Requisitos

Para instalar o Charles, será necessário um ambiente com os seguintes requisitos:

* [**Kubernetes**](https://kubernetes.io/docs/setup/)
* \*\*\*\*[**Helm** ](https://helm.sh/docs/intro/install/)\*\*\*\*
* \*\*\*\*[**Istio**](https://istio.io/archive/) ****\(versão &gt;= 1.7 e [_**sidecar injector habilitado**_](https://istio.io/latest/docs/setup/additional-setup/sidecar-injection/#automatic-sidecar-injection) no namespace de deploy das suas aplicações\).
* \*\*\*\*[**Prometheus**](https://prometheus.io/docs/prometheus/latest/getting_started/)**,** caso ****queira utilizar [**métricas**](../../referencia/metricas/)**.**

## Recursos mínimos

Os recursos mínimos considerando apenas a instalação do Charles são:

* **Microk8s**: 2GB de RAM; 
* **Minikube**: 4GB de RAM;
* **Cluster**: 2GB de RAM.

## Próximos passos 

Nesta seção, você viu os componentes, pré-requisitos e recursos mínimos para instalar o Charles. Para continuar a instalação veja: 

{% page-ref page="instalando-via-helm.md" %}

{% page-ref page="acessando-o-charles.md" %}

