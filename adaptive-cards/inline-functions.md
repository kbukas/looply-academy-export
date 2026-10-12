---
description: >-
  The Adaptive Card Designer has a large selection of functions for inline
  formatting of data.
---

# Inline Functions

The Adaptive Card Designer is great for formatting or doing complex calculations using in-line functions directly on the elements you have used to create your card. This could be a complex mathematical formula where the answer is dynamically calculated and displayed directly in a [**TextBlock**](building-adaptive-cards/content-elements.md#textblock) or format a timestamp into an easier-to-read format as soon as the card is rendered. These inline functions follow the same principles from the [**Data Binding**](data-binding.md) and/or the [**Conditional Rendering**](conditional-rendering.md) sections.

{% hint style="info" %}
Note: As this follows the same principles as [**Data Binding**](data-binding.md) and [**Conditional Rendering**](conditional-rendering.md), it is highly recommended to implement these inline functions directly into the [**Card Payload Editor**](building-adaptive-cards/#card-payload-editor). This is the easiest approach.
{% endhint %}

There is a wide variety of Inline Functions available to use, [**see here**](https://learn.microsoft.com/en-us/azure/bot-service/adaptive-expressions/adaptive-expressions-prebuilt-functions?view=azure-bot-service-4.0).

## Format Timestamp Example

The following example will describe how to convert a date/timestamp from a `DD-MM-YYYY hh:mm:ss` format to a `YYYY-MM-DD` format as commonly seen in SAP. We can do all this using inline functions within the Adaptive Card Designer. Below is a sample JSON from a Looply Workflow:

```json
{
    "payload": {
        "output": {
            "date": "03/15/2018 12:00:00" // date to format
        },
        "description": "Request"
    }
}
```

Next is the Adaptive Card JSON which will format this date into `YYYY-MM-DD` format.

```json
{
    "type": "AdaptiveCard",
    "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
    "body": [
        {
            "type": "TextBlock",
            "wrap": true,
            // inline function
            "text": "${formatDateTime($root.payload.output.date, 'yyyy-MM-dd')}"
        }
    ],
    "version": "1.5"
}
```

{% hint style="info" %}
Note: Different inline functions will require different parameters but will always be triggered the same if wrapped in `${}.`
{% endhint %}
