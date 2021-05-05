---
description: >-
  In this section, you will find the steps to configure Charles in your
  pipeline.
---

# Integrating Charles to your CI

## Why integrate Charles into your Continuous Delivery pipeline?

Integrations speed up your team and the Continuous Delivery \(CD\) allows you to take the code stored in the repository and deliver it continuously to production \(or any other environment\). 

Setting up a CD creates a quick and effective process to put your product to market before the competition, and it allows your team to launch new features and bug fixes in order to make your client happy with the result. 

## Prerequisites

To integrate Charles C.D. into your pipeline, you will need to know some information. Check out below what they are and how to get them:

* **`x-charles-token`**: It is a hash created when a systemic token is generated. If it has lost its value, it is possible to regenerate this information. **See more details in the systemic token section.**
* **`x-workspace-id`**: This Id represents the workspace where your environment settings and circles are. [**Copy the ID in the existing menu when viewing the workspace.**](creating-your-first-module/#how-to-obtain-the-component-identifier)\*\*\*\*
* **`module.id`**: This Id represents the project registered with Charles. [**Copy the ID in the existing menu when viewing the module.**](creating-your-first-module/#how-do-i-get-my-module-identifier)\*\*\*\*
* **`component.id`**: This identifier represents the component and it [**can be found in the module details**](creating-your-first-module/#how-to-obtain-the-component-identifier).
* **`component.version`**: It is where you inform the name of the tag of your component's image.
* **`component.artifact`**:  This is the name of the artifact. For example:  {YOUR-REGISTRY-URL}-{YOUR-IMAGE-NAME}:{YOUR-TAG-NAME}.
* **`circle.id`**: This Id represents the circle registered in Charles. [**Copy the ID in the existing menu when viewing the circle.**](../reference/circles.md#how-to-get-my-circles-identifier)\*\*\*\*
* **`build.id`**: This Id represents the deployment's composition created in the first request mentioned below. This information is returned as the value of the ID key in the JSON format response.

## How to integrate?

You can do this integration using a **systemic token** and a sequence of HTTPS requests. See below the two steps:

{% api-method method="post" host="https://charles.info.example" path="/moove/v2/builds/compose" %}
{% api-method-summary %}
Step 1: Create deployment composition \(aka build\)
{% endapi-method-summary %}

{% api-method-description %}
This request creates a composition that represents your release in a circle. You can mix two different versions with several components.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="x-charles-token" type="string" required=false %}
Token.
{% endapi-method-parameter %}

{% api-method-parameter name="x-workspace-id" type="string" required=true %}
Workspace's identifier.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="releaseName" type="string" required=true %}
Release name.
{% endapi-method-parameter %}

{% api-method-parameter name="modules" type="array" required=true %}
List of modules.
{% endapi-method-parameter %}

{% api-method-parameter name="module" type="object" required=true %}
An object that represents the modules.
{% endapi-method-parameter %}

{% api-method-parameter name="module.id" type="string" required=true %}
Module's identifier.
{% endapi-method-parameter %}

{% api-method-parameter name="module.components" type="array" required=true %}
List of components that composes the deployment.
{% endapi-method-parameter %}

{% api-method-parameter name="component" type="object" required=true %}
Module's component object.
{% endapi-method-parameter %}

{% api-method-parameter name="component.id" type="string" required=true %}
Component's identifier.
{% endapi-method-parameter %}

{% api-method-parameter name="component.version" type="string" required=true %}
Image tag name.
{% endapi-method-parameter %}

{% api-method-parameter name="component.artifact" type="string" required=true %}
Artifact name. For example {YOUR-REGISTRY-URL}-{YOUR-IMAGE-NAME}:{YOUR-TAG-NAME}
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=201 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
   "id":"0000053e-14cc-4bae-85df-364a70eb0000",
   "author":{
      "id":"00000afe-aa7a-4536-be1b-34eaad4c0000",
      "name":"Charles Admin",
      "email":"admin@email.com"
   },
   "createdAt":"2021-04-19 12:08:54",
   "features":[
      {
         "id":"17c28af1-d7bf-4c8c-9895-ab4944fb5a9e",
         "name":"release-test-0.1",
         "branchName":"release-test-0.1",
         "authorId":"00000afe-aa7a-4536-be1b-34eaad4c0000",
         "authorName":"Charles Admin",
         "modules":[
            {
               "id":"00000df6-6c34-4410-9bea-77ee56900000",
               "name":"ZupIT/charlescd",
               "gitRepositoryAddress":"https://github.com/zupit/charlescd",
               "helmRepository":"{HELM_URL}",
               "createdAt":"2020-11-20 13:11:31",
               "components":[
                  {
                     "id":"00000143-8208-4f95-9986-10b909c00000",
                     "name":"charlescd-ui",
                  },
                  {
                     "id":"000009ea-ada9-40fd-bddf-51c921200000",
                     "name":"charlescd-moove"
                  }
               ]
            }
         ],
         "createdAt":"2021-04-19 12:08:54",
         "branches":[
            "https://github.com/zupit/charlescd/tree/release-test-0.1"
         ]
      }
   ],
   "tag":"release-test-0.1",
   "status":"BUILT",
   "deployments":[]
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

Check out an example of the request in **CURL** format:

```text
curl 'https://charlescd.api.com/moove/v2/builds/compose' \
  -H 'x-workspace-id: 000009c4-9f58-4236-9936-ffcb04b00000' \
  -H 'x-charles-token: {YOUR_SYSTEM_TOKEN}' \
  -H 'content-type: application/json' \
  --data-binary '{
   "modules":[
      {
         "id":"00000f6-6c34-4410-9bea-77ee56900000",
         "components":[
            {
               "id":"000009ea-ada9-40fd-bddf-51c921200000",
               "version":"{YOUR-TAG-NAME}",
               "artifact":"{YOUR-REGISTRY-URL}-{YOUR-IMAGE-NAME}:{YOUR-TAG-NAME}"
            },
            {
               "id":"00000143-8208-4f95-9986-10b909c00000",
               "version":"{YOUR-TAG-NAME}",
               "artifact":"{YOUR-REGISTRY-URL}-{YOUR-IMAGE-NAME}:{YOUR-TAG-NAME}"
            }
         ]
      }
   ],
   "releaseName":"release-test-0.1"
}'
```



{% api-method method="post" host="https://charles.info.example" path="/moove/v2/deployments" %}
{% api-method-summary %}
Step 2: Deploy the release created in the previous request in a circle.
{% endapi-method-summary %}

{% api-method-description %}
This request implements the composite release, previously created in a circle.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="x-workspace-id" type="string" required=true %}
Workspace's Identifier.
{% endapi-method-parameter %}

{% api-method-parameter name="x-charles-token" type="string" required=true %}
Token.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="circleId" type="string" required=true %}
Identifier of the circle that will receive the deployment.
{% endapi-method-parameter %}

{% api-method-parameter name="buildId" type="string" required=true %}
Build identifier created in the previous request.Identificador do build criado na requisição anterior.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=201 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
   "id":"0000070c-1ed8-4d33-ad91-04ea50100000",
   "author":{
      "id":"00000afe-aa7a-4536-be1b-34eaad400000",
      "name":"Charles Admin",
      "email":"admin@email.com"
   },
   "createdAt":"2021-04-19 12:08:54",
   "deployedAt":null,
   "circle":{
      "id":"000006b3-6e04-4c48-aca9-c9297e100000",
      "name":"Circle name",
      "author":{
         "id":"00000afe-aa7a-4536-be1b-34eaad400000",
         "name":"Charles Admin",
         "email":"admin@email.com"
      },
      "createdAt":"2021-04-15 17:25:56",
      "matcherType":"REGULAR",
      "rules":{
         "type":"CLAUSE",
         "clauses":[
            {
               "type":"RULE",
               "content":{
                  "key":"email",
                  "value":[
                     "test"
                  ],
                  "condition":"EQUAL"
               }
            }
         ],
         "logicalOperator":"OR"
      },
      "importedAt":null,
      "importedKvRecords":0
   },
   "buildId":"0000053e-14cc-4bae-85df-364a70e00000",
   "tag":"{IMAGE_TAG_NAME}",
   "status":"DEPLOYING",
   "artifacts":[
      {
         "id":"00000652-e4fb-41c7-a6da-33bee0600000",
         "artifact":"{YOUR-REGISTRY-URL}-{YOUR-IMAGE-NAME}:{YOUR-TAG-NAME}",
         "version":"{IMAGE_TAG_NAME}",
         "createdAt":"2021-04-19 12:08:54",
         "componentName":"charlescd-ui",
         "moduleName":"ZupIT/charlescd"
      },
      {
         "id":"000000d2-4a50-408a-be38-c8e932200000",
         "artifact":"{YOUR-REGISTRY-URL}-{YOUR-IMAGE-NAME}:{YOUR-TAG-NAME}",
         "version":"{IMAGE_TAG_NAME}",
         "createdAt":"2021-04-19 12:08:54",
         "componentName":"charlescd-moove",
         "moduleName":"ZupIT/charlescd"
      }
   ]
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

Check out a **CURL** format example:

```text
curl 'https://charlescd.api.com/moove/v2/deployments' \
  -H 'x-workspace-id: 000009c4-9f58-4236-9936-ffcb04b00000' \
  -H 'x-charles-token: {YOUR_SYSTEM_TOKEN}' \
  -H 'content-type: application/json' \
  --data-binary '{
   "buildId":"82a7d53e-14cc-4bae-85df-364a70eb9df7",
   "circleId":"1611b6b3-6e04-4c48-aca9-c9297e18d66d"
}'
```

You can follow up on the deployments' completion using [**Webhooks**](defining-a-workspace/web.md).

