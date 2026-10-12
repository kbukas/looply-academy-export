---
description: Adaptive Card related endpoints
hidden: true
---

# Adaptive Card API

## getCardById

<mark style="color:blue;">`GET`</mark> `https://api.looply.io/v1/adaptive-cards/getCardById`

Returns the Adaptive Card JSON Payload and related data.

#### Query Parameters

| Name                                            | Type   | Description         |
| ----------------------------------------------- | ------ | ------------------- |
| card\_id<mark style="color:red;">\*</mark>      | String | id for the card     |
| card\_version<mark style="color:red;">\*</mark> | String | version of the card |

#### Headers

| Name                                        | Type   | Description  |
| ------------------------------------------- | ------ | ------------ |
| x-api-key<mark style="color:red;">\*</mark> | String | Your API Key |

{% tabs %}
{% tab title="200: OK Success" %}
Example Payload

```json
{
  "message": "success",
  "item": {
    "organization_id": "****",
    "created_by": "****",
    "status": "DRAFT",
    "modified_by": "****",
    "workflow_version": 1,
    "card_id": "****",
    "workflow_id": "****",
    "card_name": "Card Name",
    "modified_on": 1697454380810,
    "status_version": "DRAFT-1",
    "latest": "true",
    "card_payload": {
      // Adaptive Card JSON Payload
    },
    "card_version": 1,
    "workspace_id": "****",
    "created_on": 1695034193817
  }
}

```
{% endtab %}

{% tab title="401: Unauthorized You don't have access to this adaptive card." %}

{% endtab %}

{% tab title="403: Forbidden Incorrect or No API Key Supplied" %}

{% endtab %}

{% tab title="404: Not Found Card does not exist." %}

{% endtab %}

{% tab title="500: Internal Server Error Server side error." %}

{% endtab %}

{% tab title="400: Bad Request Incorrect or missing Query Parameters" %}

{% endtab %}
{% endtabs %}

## getLatestActiveCard

<mark style="color:blue;">`GET`</mark> `https://api.looply.io/v1/adaptive-cards/getLatestActiveCard`

Returns the latest active card.

#### Query Parameters

| Name                                       | Type   | Description     |
| ------------------------------------------ | ------ | --------------- |
| card\_id<mark style="color:red;">\*</mark> | String | id for the card |

{% tabs %}
{% tab title="400: Bad Request Incorrect or missing Query Parameters" %}

{% endtab %}

{% tab title="200: OK Success" %}
Example Payload:

```json
{
  "message": "success",
  "item": {
    "organization_id": "****",
    "created_by": "****",
    "status": "DRAFT",
    "modified_by": "****",
    "workflow_version": 1,
    "card_id": "****",
    "workflow_id": "****",
    "card_name": "Card Name",
    "modified_on": 1697454380810,
    "status_version": "DRAFT-1",
    "latest": "true",
    "card_payload": {
      // Adaptive Card JSON Payload
    },
    "card_version": 1,
    "workspace_id": "****",
    "created_on": 1695034193817
  }
}
```
{% endtab %}

{% tab title="401: Unauthorized You don't have access to this adaptive card." %}

{% endtab %}

{% tab title="403: Forbidden Incorrect or No API Key Supplied" %}

{% endtab %}

{% tab title="404: Not Found Card does not exist." %}

{% endtab %}

{% tab title="500: Internal Server Error Server side error." %}

{% endtab %}
{% endtabs %}

## getOrganizationCards

<mark style="color:blue;">`GET`</mark> `https://api.looply.io/v1/adaptive-cards/getOrganizationCards`

Returns a list of all the cards created in the organization. Will return a `lastKey` attribute. Use this to get the next page of data.&#x20;

Limit can be added to the request, 30 is the max.

#### Query Parameters

| Name          | Type   | Description                                    |
| ------------- | ------ | ---------------------------------------------- |
| limit         | String | Number of items to return per page. Default 20 |
| card\_id      | String | Pagination Key from the `lastKey` attribute    |
| card\_version | String | Pagination Key from the `lastKey` attribute    |

#### Headers

| Name                                        | Type   | Description  |
| ------------------------------------------- | ------ | ------------ |
| x-api-key<mark style="color:red;">\*</mark> | String | Your API Key |

{% tabs %}
{% tab title="200: OK Success" %}
Example Payload:

```json
{
  "message": "success",
  "count": 20,
  "items": [
    {
      "card_name": "Card name",
      "status_version": "DRAFT-1",
      "organization_id": "****",
      "latest": "true",
      "created_by": "****",
      "status": "DRAFT",
      "card_version": 1,
      "card_id": "****",
      "workspace_id": "****",
      "created_on": 1693234726646
    }
  ],
  "lastKey": {
    "organization_id": "****",
    "card_id": "****",
    "card_version": 1
  }
}

```
{% endtab %}

{% tab title="500: Internal Server Error Server side Error" %}

{% endtab %}
{% endtabs %}
