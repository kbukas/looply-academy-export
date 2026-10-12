---
description: Bind data from your Looply Workflow directly into your Adaptive Card.
---

# Data Binding

The data binding feature from the Adaptive Card designer allows the binding of any JSON attribute from the [**Sample Data Editor**](building-adaptive-cards/#sample-data-editor) to an element on an Adaptive Card.&#x20;

### Linking a Workflow to an Adaptive Card

When adding an Adaptive Card to a Workflow, if you click "Create new card" this will create a new Card but will also link the Workflow Schema to the Adaptive Card allowing you to bind for example the result from an API request to a [TextBlock](building-adaptive-cards/content-elements.md#textblock) on your Adaptive Card. Find a more in-depth explanation from the [Using Integrations Section](../workflows/using-integrations/) or check out [Chapter 1 of our Building Adaptive Cards tutorial](../tutorials/building-adaptive-cards.md).

### Simple Data Binding

Whilst in the Adaptive Card Designer you will notice a  [**Sample Data Editor**](building-adaptive-cards/#sample-data-editor)**,** this holds the data that can be bound to the Adaptive Card you are creating. Data can be added to this code editor or it can be pulled from the workflow schema where this card is bound. Below is a sample [**Sample Data Editor**](building-adaptive-cards/#sample-data-editor) that takes data from a **Workflow Schema**:

{% code fullWidth="false" %}
```json
{
    "payload": {
        "output": {
            "workflow_version": "1",
            "workspace_id": "****",
            "workflow_process_id": "****",
            "workflow_id": "****",
            "looply_sap_profile_settings": {
                "requires_sap_profile": false
            },
            "executor": "****",
            "organization_id": "****",
            "workflow_execution_id": "****",
            "workflow_trigger_type": "REQUEST"
        },
        "description": "Request"
    },
    "WorkflowStartState": null,
    "function_1": {
        "output": "hello world",
        "description": "My Function"
    }
}
```
{% endcode %}

Data binding can be done on a wide variety of properties ranging from the Text property of a [**TextBlock**](building-adaptive-cards/content-elements.md#textblock) to the choices of an [**Input.ChoiceSet**](building-adaptive-cards/input-elements.md#input.choiceset)**.**

Binding data from the [**Sample Data Editor**](building-adaptive-cards/#sample-data-editor) is easy. You can do it via any of these 3 ways:

1. When an element on the [**Adaptive Card Canvas**](building-adaptive-cards/#adaptive-card-canvas) has been selected, some elements like the [TextBlock](building-adaptive-cards/content-elements.md#textblock) will show you a "Bind..." button. Clicking on this will pull up a dropdown menu showing you all the attributes you can bind to the [TextBlock](building-adaptive-cards/content-elements.md#textblock). This dropdown is generated via the data supplied in the [**Sample Data Editor**](building-adaptive-cards/#sample-data-editor)**.**
2. When an element on the [**Adaptive Card Canvas**](building-adaptive-cards/#adaptive-card-canvas) has been selected, the [**Element Properties Toolbar**](building-adaptive-cards/#element-properties-toolbar) should have all the customisation options for the respective field. Notice that certain fields have a button displaying **"..."**, clicking on this will pull up a dropdown menu showing you all the attributes you can bind to this particular property. Like option 1, this dropdown is generated via the data supplied in the [**Sample Data Editor**](building-adaptive-cards/#sample-data-editor)**.**
3. You can manually type in the binding directly into the [**Card Payload Editor**](building-adaptive-cards/#card-payload-editor)**.**&#x20;

{% hint style="warning" %}
Note: Please note that if a binding is incorrectly typed directly into the [**Card Payload Editor**](building-adaptive-cards/#card-payload-editor), the adaptive card canvas may not appear.
{% endhint %}

Binding will appear in the [**Card Payload Editor**](building-adaptive-cards/#card-payload-editor) as shown below:

```json
{
    "type": "AdaptiveCard",
    "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
    "body": [
        {
            "type": "TextBlock",
            "wrap": true,
            "text": "${$root.payload.output.workflow_id}" // data binding
        }
    ],
    "version": "1.5"
}
```

{% hint style="warning" %}
Note: Manually typing in data bindings must follow the above syntax.
{% endhint %}

#### [Check out Chapter 7 of our Building Adaptive Cards tutorial!](../tutorials/building-adaptive-cards.md)

### Binding JSON Arrays

Data in our [**Sample Data Editor**](building-adaptive-cards/#sample-data-editor) may contain arrays of strings or arrays of JSON objects. Binding these structures to our Adaptive Card is a little different from the above [**Simple Data Binding**](data-binding.md#simple-data-binding) which follows a more one-to-one approach.&#x20;

{% hint style="warning" %}
Note: Manually typing data bindings from an array is highly advised. Using the [**Element Properties Toolbar**](building-adaptive-cards/#element-properties-toolbar) will result in incorrect mappings for array items.
{% endhint %}

If the data within the [**Sample Data Editor**](building-adaptive-cards/#sample-data-editor) looked as follows:

```json
{
    "payload": {
        "output": {
            "process_id": "****",
            "workflow_id": "****",
            "workflow_execution_id": "****",
            "version": "5",
            "scenario_id": "****",
            "workflow_version": "1",
            "workspace_id": "****",
            "util_data": [ // array of JSON objects to bind
                {
                    "title": "Claim not approved",
                    "value": "1"
                },
                {
                    "title": "Query with claim",
                    "value": "2"
                },
                {
                    "title": "Incorrect grade / SCP claimed",
                    "value": "3"
                },
                {
                    "title": "Incorrect payment type claimed",
                    "value": "4"
                }
            ],
            "organization_id": "****",
            "workflow_trigger_type": "REQUEST",
            "recipient": "email@example.COM",
            "step": "****"
        },
        "description": "Request"
    }
}
```

#### Displaying Singular Items.

Displaying only the 3rd `title` from our `util_data` array is straightforward and replicates process 3 from [Simple Data Binding](data-binding.md#simple-data-binding).&#x20;

```json
{
    "type": "AdaptiveCard",
    "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
    "version": "1.5",
    "body": [
        {
            "type": "TextBlock",
            "wrap": true,
            "text": "${$root.payload.output.util_data[2].title}" // data binding
        }
    ]
}
```

#### Displaying Lists

If we want to display a [**TextBlock**](building-adaptive-cards/content-elements.md#textblock) for each of the `title` values in our `util_data` array we need to manually type this binding into our [**Card Payload Editor**](building-adaptive-cards/#card-payload-editor) against our [**TextBlock**](building-adaptive-cards/content-elements.md#textblock)**.** A "**data context**" has to be established on our [**TextBlock**](building-adaptive-cards/content-elements.md#textblock) - this means that the [**TextBlock**](building-adaptive-cards/content-elements.md#textblock) is only observing data within a certain attribute within our [**Sample Data Editor**](building-adaptive-cards/#sample-data-editor)**.** Due to a data context being applied to the [**TextBlock**](building-adaptive-cards/content-elements.md#textblock)**,** our binding will appear shorter than the above [Simple Data Binding](data-binding.md#simple-data-binding) approach. Below is how we can display 8 TextBlocks, where each element points to a different `title` in our `util_data` array.

```json
{
    "type": "AdaptiveCard",
    "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
    "version": "1.5",
    "body": [
        {
            "type": "TextBlock",
            "wrap": true,
            "$data": "${$root.payload.output.util_data}", // data context
            "text": "${title}" // data binding
        }
    ]
}
```

{% hint style="info" %}
Note: Binding data using a "**data context**" will stop the Adaptive Card Designer from immediately showing the final rendered card. You will have to use the **Test Card** action to see the final result - [Card Toolbar](building-adaptive-cards/#card-toolbar) then: View>Test Card.
{% endhint %}

#### Using JSON Arrays in Input.ChoiceSets

You can bind data directly to the `choices` attribute of an [Input.ChoiceSet](building-adaptive-cards/input-elements.md#input.choiceset). This is particularly useful if the choices are dynamic and could change.&#x20;

```json
{
    "type": "AdaptiveCard",
    "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
    "version": "1.5",
    "body": [
        {
            "type": "Input.ChoiceSet",
            "choices": "${$root.payload.output.util_data}", // data binding
            "placeholder": "Choices"
        }
    ]
}
```

{% hint style="info" %}
Note: This works due to the structure of `util_data` replicating that of an [Input.ChoiceSet](building-adaptive-cards/input-elements.md#input.choiceset) `choices` attribute array. Always test your card using the **Test Card** action in the **View** menu of the [Card Toolbar](building-adaptive-cards/#card-toolbar) to check the binding has worked.
{% endhint %}

#### [Check out Chapter 8 of our Building Adaptive Cards tutorial!](../tutorials/building-adaptive-cards.md)
