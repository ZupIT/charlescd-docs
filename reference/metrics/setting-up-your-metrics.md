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

See below the details of the tools Charles will be able to read.

{% tabs %}
{% tab title="Prometheus" %}
Prometheus is an open-source system for monitoring and alerting toolkit. It is the main monitoring recommendation on [**Cloud Native Computing Foundation**](https://cncf.io/)**.**

{% hint style="info" %}
If you want to know more about Prometheus, check out [**their documentation**](https://prometheus.io/)**.**
{% endhint %}

In order for Prometheus to be able to read and store metrics data, you have to configure it.

To do so, it's necessary to add the job below so it will read Istio's generated metrics. Just configure by editing the **prometheus.yml** file into your **Prometheus configMap**.

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
If you want to know more about Prometheus and Kubernetes service discovery,  check out [**their documentation**](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#kubernetes_sd_config).
{% endhint %}

### Metadata

‌Each metric has a metadata range that allows a variety of filter and analysis types to be created. More metadata was added to Istio and you can see them described in the table below:

| Metadata | Description | Type |
| :--- | :--- | :--- |
| destination\_component | Value on the label 'app' of the pod that received the request or unknown if there is no information about it. | Text |
| circle\_source | Circle label injected into any Kubernetes pods. | Text |
| response\_code | HTTP status of the response. | Numeric |
{% endtab %}

{% tab title="Google Analytics" %}
Google Analytics is one of the data sources that Charles can connect to read your metrics. 

To be able to use in your metrics group, you will need

* A Google account and the Analytics configured.

If you want to use Charles to analyze your Google Analytics data, you need to add a new metric with your circle ID \(**renaming it as circle\_source**\) in your metrics label.

{% hint style="info" %}
For more information about it, check out [**their documentation**](https://developers.google.com/analytics/devguides/reporting/core/v4)**.**
{% endhint %}
{% endtab %}
{% endtabs %}

