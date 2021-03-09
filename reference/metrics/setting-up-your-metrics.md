---
description: >-
  In this section, you will find more information about how to set up your
  metrics using Charles.
---

# Setting up your metrics

Charles' metrics configuration is performed on Istio and on your own metrics provider. See more details below. 

## Istio Configuration

Metrics related to circle requests are quantified and exposed by Istio, so it's necessary to configure it to get information about each circle.

{% hint style="danger" %}
The configuration in this section can be done starting with Istio =&gt;1.7 versions. 
{% endhint %}

## Configuring your metrics' tool

After you finish your Istio configuration it is necessary to configure your metrics tool.

See below the details of the tool Charles will be able to read.

{% tabs %}
{% tab title="Prometheus" %}
Prometheus is an open-source system for monitoring and alerting toolkit. It is the main monitoring recommendation on [**Cloud Native Computing Foundation**](https://cncf.io/)**.**

{% hint style="info" %}
If you want to know more about Prometheus, check out [**their documentation**](https://prometheus.io/)**.**
{% endhint %}

In order for Prometheus to be able to read and store metrics data, we must configure it.

To do so, it's necessary to add the job below so it will read Istio's generated metrics.

{% hint style="warning" %}
It is important to remember that all these configurations consider that your Prometheus is on the same Kubernetes cluster as your Istio and the rest of your applications.
{% endhint %}

```yaml
global:
      scrape_interval:     15s
      scrape_timeout:      10s
      evaluation_interval: 15s
    scrape_configs:
      - job_name: kubernetes-pods
      kubernetes_sd_configs:
      - role: pod
      relabel_configs:
      - action: keep
        regex: true
        source_labels:
        - __meta_kubernetes_pod_annotation_prometheus_io_scrape
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

{% hint style="warning" %}
Stay tuned about the configuration **namespaces**, the configured value must be the same installed on your Istio.
{% endhint %}
{% endtab %}
{% endtabs %}

### Metadata

Each metric has a metadata range that allows a variety of filter and analysis types to be created. These metadata are described in the table below:

| Metadata | Description | Type | Metrics that are present |
| :--- | :--- | :--- | :--- |
| destination\_component | Value on the label 'app' of the pod that received the request or unknown if there is no information about it. | Text | istio\_charles\_request\_total, istio\_charles\_request\_duration\_seconds |
| circle\_source | Circle label injected into any kubernetes pods. | Text | istio\_charles\_request\_total, istio\_charles\_request\_duration\_seconds |
| response\_code | HTTP status of the response. | Numeric | istio\_charles\_request\_total, istio\_charles\_request\_duration\_seconds |

