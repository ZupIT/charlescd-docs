---
description: >-
  Nesta seção, você encontra detalhe de como configurar suas métricas no
  Charles.
---

# Configurando as métricas

A configuração de métricas no Charles é realizada no **Istio** e no seu **próprio provedor**. Veja os detalhes abaixo. 

## Configurando Istio

As métricas relacionadas às requisições de cada círculo podem ser quantificadas e expostas pelo Istio.

{% hint style="danger" %}
A configuração pode ser feita a partir da versão =&gt;1.7 do Istio.
{% endhint %}

## Configurando sua própria ferramenta de métricas

Depois de habilitar o Istio, você precisa configurar sua ferramenta para que ela possa ler as métricas expostas.

Veja abaixo os detalhes das **ferramentas compatíveis com o Charles**.

{% tabs %}
{% tab title="Prometheus" %}
O Prometheus é uma ferramenta de código aberto focada em monitoramento e alertas. É considerada a principal recomendação para monitoramento do [**Cloud Native Computing Foundation**](https://cncf.io/), além de uma das principais ferramentas do mercado.

{% hint style="info" %}
Se quiser saber mais, sugerimos a [**doc oficial**](https://prometheus.io/).
{% endhint %}

É preciso configurar o Prometheus para que ele consiga ler e armazenar os dados das métricas habilitadas, conforme o tutorial que explicamos no início.

Para fazer isso, é necessário adicionar o job abaixo para que leia a métrica gerada pelo Istio. Para configurar, edite o arquivo **prometheus.yml** no seu **Prometheus configMap**.

{% hint style="warning" %}
É importante lembrar que, para que essas configurações funcionem, é necessário que seu Prometheus esteja no mesmo cluster de Kubernetes que o Istio e o restante das suas aplicações.
{% endhint %}

Basta adicionar o job abaixo para realizar a configuração.

```yaml
global:
      scrape_interval:     15s
      scrape_timeout:      10s
      evaluation_interval: 15s
    scrape_configs:
      - job_name: charles-metrics
        honor_timestamps: true
        scrape_interval: 5s
        scrape_timeout: 5s
        metrics_path: /v1/metrics
        scheme: http
        kubernetes_sd_configs:
        - role: endpoints
          namespaces:
          names:
          - namespace1
          - namespace2
        relabel_configs:
        - separator: ;
          regex: _meta_kubernetes_pod_label(.+)
          replacement: $1
          action: labelmap
        - source_labels: [__meta_kubernetes_pod_label_circleId]
          separator: ;
          regex: (.*)
          target_label: circle_source
          replacement: $1
          action: replace
        - source_labels: [__meta_kubernetes_pod_label_version]
          separator: ;
          regex: (.*)
          target_label: version
          replacement: $1
          action: replace
        - source_labels: [__meta_kubernetes_pod_name]
          separator: ;
          regex: (.*)
          target_label: pod_name
          replacement: $1
          action: replace
      - job_name: kubernetes-pods
        kubernetes_sd_configs:
        - role: pod
        relabel_configs:
        - action: keep
          regex: true
          source_labels:
          - __meta_kubernetes_pod_annotation_pryometheus_io_scrape
        - action: replace
          regex: (.+)
          source_labels:
          - __meta_kubernetes_pod_annotation_prometheus_io_path
          target_label: __metrics_path__
        - action: replace
          regex: ([^:]+)(?::\d+)?;(\d+)
          replacement: $1:$2
          source_labels:
          - __address__
          - __meta_kubernetes_pod_annotation_prometheus_io_port
          target_label: __address__      
        - action: replace
          source_labels:
          - __meta_kubernetes_namespace
          target_label: kubernetes_namespace
        - action: replace
          source_labels:
          - __meta_kubernetes_pod_label_circleId
          target_label: circle_source
        - action: replace
          source_labels:
          - __meta_kubernetes_pod_label_component
          target_label: destination_component      
        - action: replace
          source_labels:
          - __meta_kubernetes_pod_name
          target_label: kubernetes_pod_name
        - action: drop
          regex: Pending|Succeeded|Failed
          source_labels:
          - __meta_kubernetes_pod_phase
```

{% hint style="info" %}
Mude o nome do **namespace** para um onde sua aplicação está deployada. 
{% endhint %}

{% hint style="warning" %}
Para saber mais sobre o serviço de discovery do Prometheus e Kubernetes, [**veja a documentação**](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#kubernetes_sd_config).
{% endhint %}

### Metadados

A partir de cada métrica, é possível extrair uma série de metainformações, ou seja, de atributos ou informações complementares a essas métricas e que podem ser obtidas com diversos tipos de filtros e análises.

‌Cada métrica possui uma faixa de metadado que permite uma variedade de filtros e tipos de análises a serem criadas. Foram adicionados mais metadados ao Istio e ele são descritos na tabela abaixo: 

| Metadado | Descrição | Tipo |
| :--- | :--- | :--- |
| destination\_component | Valor presente na label "app" da POD que recebeu a requisição ou "unknown" se a informação não estiver presente.  | Texto |
| circle\_source | Label do círculo injetado em qualquer pod do Kubernetes.   | Texto |
| response\_code | O status HTTP da resposta daquela requisição. | Número |
{% endtab %}

{% tab title="Google Analytics" %}
O Google Analytics é um dos data sources que o Charles pode conectar para ler suas métricas.

Para usá-lo no seu grupo de métricas, você precisa de: 

* Uma conta Google e o Analytics configurado.

Se você quiser usar o Charles para analizar os dados do seu Google Analytics, você precisa adicionar uma nova métrica com a ID do circulo \(**renomeando como circle\_source**\) na label da sua métrica. 

{% hint style="info" %}
Para mais informações sobre o Google Analytics, [**veja a documentação**](https://developers.google.com/analytics/devguides/reporting/core/v4).
{% endhint %}
{% endtab %}
{% endtabs %}

| Metadado | Descrição | Tipo |
| :--- | :--- | :--- |
| destination\_component | Valor presente na label "app" da POD que recebeu a requisição ou "unknown" se a informação não estiver presente | Texto |
| circle\_source | Header "x-circle-source" que é colocado pelo filtro do Envoy na interceptação de cada requisição | Texto |
| response\_code | O status HTTP da resposta daquela requisição | Númer |

