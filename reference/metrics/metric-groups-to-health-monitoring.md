---
description: 'In this section, you will find information about health monitoring.'
---

# Metric groups to health monitoring

## What is this?

When your application is deployed, it’s a good practice to check its health, confirm if the software is working fine or if it needs special care. 

You can do this with Charles, using your ****[**previously configured metrics datasource**](../../get-started/defining-a-workspace/datasource.md) to create **metric groups** that will let you know the status of your deployed image.

See more details below. 

## Requisites

To monitor your metrics you need to have:

*  Datasource configured using [**Prometheus**](https://prometheus.io/)
*  [**Istio**](https://istio.io/latest/) ****1.7 or newer versions.

## How can you monitor? 

1. You have to create your **metrics group and your own metric,** to do that follow the [**steps here**](metrics-group.md)**;**
2. In the advanced mode, execute **PromQL queries** to monitor your metrics.

{% hint style="info" %}
To know more about **PromQL,** check out their [**documentation**](https://prometheus.io/docs/prometheus/latest/querying/basics/).
{% endhint %}

## Metrics Examples 

After you have created your metric group, you can create your own metrics. Check out below some examples

{% hint style="info" %}
When you use these examples, you can create lots of metrics, for example, the proportion of errors by total requests and you can trigger an Action if this metric reach 10%.
{% endhint %}

### Latency metric

Here, for example, a metric was created to give you the average of the Latency, dividing the duration \(in milliseconds\) of requests in the last minute by the total of requests in the same time.  
  
At the end of the query, the value is multiplied by 1000 and you will get the result in seconds.

{% hint style="warning" %}
Change the **xyz** value into your query using the Circle ID value. It's valid for all examples below.
{% endhint %}

```
round (
 ( sum (
     irate (
       istio_request_duration_milliseconds_sum{circle_source="xyz"}[1m]
     )
   ) by(destination_component) /
   sum (
     irate (
       istio_request_duration_milliseconds_count{circle_source="xyz"}[1m]
     )
   ) by(destination_component)
 ) 
   * 1000
)
```

{% hint style="info" %}
After you have installed Istio and Prometheus and enabled Istio injection on your pods, your Datasource will have Istio metrics. These metrics can be used to build advanced ones. Check out more about this metric on [**Istio Standard Metrics documentation**](https://istio.io/latest/docs/reference/config/metrics/).
{% endhint %}

### Total requests metric

This metric will give you the total of requests at the last minute: 

```
ceil (
  sum (
    irate (
      istio_requests_total{circle_source="xyz"}[1m]
    )
  ) 
  by(destination_component)
) 
```

### Error requests metric

This metric will give you the requests at the last minute,  but only for response status code 404: 

```
ceil (
  sum (
    irate (
      istio_requests_total{circle_source="xyz",
      response_code="404"}[1m]
    )
  ) 
  by(destination_component)
) 
```

{% hint style="warning" %}
Define a threshold that fills what you need to monitor. 
{% endhint %}

