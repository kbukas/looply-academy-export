---
description: Render select elements depending on the cards current data payload.
---

# Conditional Rendering

Different Payloads will be used against adaptive cards depending on the results from the previous states within a Looply Workflow. Different results may require you to change the card to accommodate these changes. e.g. an error message from a previous state in the Looply Workflow.

{% hint style="info" %}
Note: Using the [**Card Payload Editor**](building-adaptive-cards/#card-payload-editor) is the easiest method to write the logic for conditional rendering.
{% endhint %}

### Overview

Each element has an **"Only show when"** attribute which is rendered as the `$when` attribute in the [**Card Payload Editor**](building-adaptive-cards/#card-payload-editor)**.** If this attribute is `true`, will display the element.&#x20;

Each element has an **"Initially Visible"** attribute which is rendered as the `isVisible` attribute in the [**Card Payload Editor**](building-adaptive-cards/#card-payload-editor)**.** If this attribute is `true`, will display the element when the card first loads. We will not go into detail with this option as it is not as powerful as the `$when` attribute as described above.

Adding a condition to trigger a `true` or `false` value for the `$when` takes principles described in the [Data Binding](data-binding.md) section and standard logical operations commonly seen in programming languages such as JavaScript.&#x20;

{% hint style="info" %}
Note: Due to the conditions being written in a JSON format they need to be encased in a string. Checking if a string equals another will require you to use `\` to escape the quotes special character.
{% endhint %}

### Conditionally Rendering an Error Message

If you were to get an error from your Looply Workflow. We may need to display a red banner and a message about what has gone wrong. But we should only display this if we get an error otherwise we should display a success message.

An example payload from a Looply Workflow could look like the following:

```json
{
    "httpRequestSAP_1": {
        "output": {
            "headers": {
                "www-authenticate": "Bearer realm=\"SAP NetWeaver Application Server [GW1/100]\", error=\"invalid_request\", error_description=\"Both, OAuth 2.0 token endpoint and resource server require usage of TLS 1.0. Caller used HTTP instead of HTTPS\"",
                "set-cookie": "sap-usercontext=sap-client=100; path=/",
                "content-length": "0",
                "content-type": "text/html",
                "sap-server": "true"
            },
            "data": "CSRF Token fetch failed",
            "status": "400",
            "statusText": "Bad Request"
        },
        "description": "SAP Update"
    }
}
```

We want to conditionally render the message at `httpRequestSAP_1.output.data` if the status is `400` or `500`. Below is an example of the card payload from the [**Card Payload Editor**](building-adaptive-cards/#card-payload-editor) to make this happen:

```json
{
    "type": "AdaptiveCard",
    "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
    "version": "1.5",
    "body": [
        {
            "type": "Container",
            "items": [
                {
                    "type": "TextBlock",
                    "text": "SUCCESS",
                    "wrap": true
                }
            ],
            "style": "good",
            // render the success message if the status does not equal 400
            "$when": "${$root.httpRequestSAP_1.output.status != \"400\"}" 
        },
        {
            "type": "Container",
            "items": [
                {
                    "type": "TextBlock",
                    "text": "ERROR - ${$root.httpRequestSAP_1.output.data}",
                    "wrap": true
                }
            ],
            "style": "attention",
            // render the error message if the status equals 400 or 500
            "$when": "${$root.httpRequestSAP_1.output.status == \"400\" || $root.httpRequestSAP_1.output.status == \"500\"}"
        }
    ]
}
```

All conditions are encased in `${}` much like that of [data binding](data-binding.md).&#x20;

{% hint style="info" %}
Note: Similar to the binding in the [Binding JSON Arrays](data-binding.md#binding-json-arrays) section. We can only see the result by testing the card using the Test Card action in the View menu from the [Card Toolbar](building-adaptive-cards/#card-toolbar).
{% endhint %}

### Logical Operators

As described above, the logical operators that are available are similar to those found in some programming languages. Below is a list:

| Operator | Operation                |
| -------- | ------------------------ |
| &&       | AND                      |
| \|\|     | OR                       |
| \\       | Escape Special Character |
| !=       | Not Equals               |
| ==       | Equals                   |

### Prebuilt Functions

You can make use of a number of prebuilt functions in your conditional rendering logic. For more information, visit [this link](https://learn.microsoft.com/en-us/azure/bot-service/adaptive-expressions/adaptive-expressions-prebuilt-functions?view=azure-bot-service-4.0).&#x20;

#### [Check out Chapter 9 of our Building Adaptive Cards tutorial!](../tutorials/building-adaptive-cards.md)
