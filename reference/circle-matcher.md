---
description: >-
  In this section, you will find more information about how Circle Matcher works
  on Charles.
---

# Circle Matcher

Circle Matcher is a resource that allows you to validate if your [**circles**](circles.md) are incoherent segmentations. You can also use it in your applications to determine in which circle your users fit better.

{% hint style="info" %}
One good practice is to always make this identification when a user logs in to the application. However, this can be changed according to your business needs.
{% endhint %}

For more information on how to configure your **Circle Matcher in a workspace**, [**see Defining Workspace section**](../get-started/defining-a-workspace/circle-matcher.md)**.** 

## Identifying circles through CharlesCD

Once you start using the interface, it's possible to notice that there are two ways to perform the circle identification. For that, access the **Circles** menu inside a **workspace** and select the icon indicated below:

![Circle Matcher identification icon ](../.gitbook/assets/chrome-capture%20%282%29.jpg)

The two ways to make this validation are:

* **Default:** in this option, you add manually keys and values to define the characteristics of a user test. Based on that, once you run the **Try**,  **you will receive all the circles related to this user.**  

![Circle identification with Default option.](../.gitbook/assets/circle-matcher-default%20%282%29.gif)

* **JSON:** similar to the default option, with the difference that here you can copy and paste in **payload field** a **JSON** of your productive environment instead of adding manually. 

![Circle identification with JSON option. ](../.gitbook/assets/circle-matcher-json%20%282%29.gif)

{% hint style="warning" %}
If you pass some information that is off the preconfigured logic conditions in the circles, the system will return indicating that the user is on _Default_ circle, on the standard version of your application.
{% endhint %}

## Circle identification through API

You can integrate with your applications the **Identify** resource on the [`charles-circle-matcher`](https://github.com/ZupIT/charlescd/tree/master/circle-matcher) module to detect the circles the user belongs to.

For example, considering the use of the parameters below to segment:

![](https://lh6.googleusercontent.com/q573-961WtpntVK8NfXXvPgzSPrxLwxjx3QXRqM3vBlHFM8nAoDkpn1KD26Zfw3_wJtjnhVldYcwRUUzhbveEvqJz6n16NQFkxi0S3hh8rk6Y7OUmWtnBOl_qJekzoymQ64mFF8k)

Once you send the identification request with some information, compatible circles will be returned.

{% api-method method="post" host="https:// api.charlescd-circle-matcher.com/identify " path="" %}
{% api-method-summary %}
Identify
{% endapi-method-summary %}

{% api-method-description %}
The method used to identify circles based on the user's characteristics
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="requestData" type="object" required=true %}
{ "state": "NY", "profession": "Lawyer", "age": 46, "city": "Stony Brook"}
{% endapi-method-parameter %}

{% api-method-parameter name="workspaceId" type="string" required=true %}
UUID
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```text
{
  "circles": [
    {
      "id": "6577ae92-648c-11ea-bc55-0242ac130003",
      "name": "NY Lawyers"
    },
    {
      "id": "6577b112-648c-11ea-bc55-0242ac130003",
      "name": "Stony Brook's Citizens"
    }
  ]
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

As the example above shows, there are circles with the given information of the user, which means that **`charlescd-circle-matcher`**will return a list with all the circles. Here, there are two circles that fit with this description: NY Lawyers e Stony Brook’s Citizens. The order of the circles returned will be by the date of creation, so the newest circle created will be the first of the list.

The requisition body is totally flexible, but it's good to remember that the keys must have the same nomenclature defined by segmentation's rules of the circle. See the case below:

![](https://lh3.googleusercontent.com/FdPVIHDFeYJCkC_6Y1P3ZOBSqmNlGkl9q2_XyIayNKQo2Mp9IXBY7PzvpzW0Mej1P9Ox8AG12QiA1H0w5uozWP1UYWafcfwXLKBOf3G-ObIVoPHtYGOlWd5Ju01uLuScqtCn8qQ1)

The **Stony Brook’s Citizens** circle was created to identify users that contain as one of its characteristics the key **`city`** and the exact value **`London`**. That means that this user won't be listed if you send a request to **`Identify`** and inform on the requisition body the information presented on the example below:

{% api-method method="post" host="https://" path="api.charlescd-circle-matcher.com/identify" %}
{% api-method-summary %}
Identify
{% endapi-method-summary %}

{% api-method-description %}
The method used to identify circles based on the user's characteristics 
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="requestData" type="object" required=true %}
{ "age": 46, "city": "London" }
{% endapi-method-parameter %}

{% api-method-parameter name="workspaceId" type="string" required=true %}
UUID
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
List of all circles the user belongs to
{% endapi-method-response-example-description %}

```
{
  "circles": [
    {
      "id": "6577ae92-648c-11ea-bc55-0242ac130003",
      "name": "Default"
    }
  ]
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

