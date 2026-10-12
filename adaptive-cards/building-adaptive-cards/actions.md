---
description: >-
  Adaptive Cards have a wide range of actions that can be triggered in a few
  different ways.
---

# Actions

## Trigger Points

There are a few ways to trigger the following actions. Options range from a standard button from inside an [**ActionSet**](content-elements.md#actionset) or an [**Image**](content-elements.md#image) **:**&#x20;

* Adding the **ActionSet** to your Adaptive Card and selecting "Add an action" from the [**Card Canvas.**](./#adaptive-card-canvas)
* Adding an [**Image**](content-elements.md#image) and selecting an **Action Type** from the dropdown menu within the [**Element Properties Toolbar**](./#element-properties-toolbar)**.**
* Selecting a column from a [**ColumnSet**](container-elements.md#columnset) and selecting an **Action Type** from the dropdown menu within the [**Element Properties Toolbar**](./#element-properties-toolbar)**.**
* Selecting a TableCell from a [**Table**](container-elements.md#table) and selecting an **Action Type** from the dropdown menu within the [**Element Properties Toolbar**](./#element-properties-toolbar)**.**
* Add actions directly onto the card by selecting the [**Card Canvas**](./#adaptive-card-canvas) and clicking "Add an action".

## Action.OpenUrl

#### Description

A simple action that will open whatever URL you put in the "Url" parameter in the [**Element Properties Toolbar**](./#element-properties-toolbar)**.**

#### **Technical Documentation**

[**Read more about Action.OpenUrl**](https://adaptivecards.io/explorer/Action.OpenUrl.html)

## Action.Submit

#### Description

Acts as the main action button for sending data back to Looply.&#x20;

Use this action for "Approving" or "Rejecting" cards or if your card contains Input Elements where the data you want to use will be sent to your Looply Workflow. When a user clicks on this action any data bound to the action is sent back to Looply as well.&#x20;

For example, a card that is of this shape:

```json
{
    "type": "AdaptiveCard",
    "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
    "version": "1.5",
    "body": [
        {
            "id": "input1",
            "type": "Input.Text",
            "placeholder": "Placeholder text"
        },
        {
            "id": "input2",
            "type": "Input.Text",
            "placeholder": "Placeholder text"
        }
    ],
    "actions": [
        {
            "style": "positive",
            "data": {
                // attach your own data here.
                "action": "APPROVE"
            },
            "title": "Approve",
            "type": "Action.Submit"
        }
    ]
}
```

The above card has 2 [**Input.Text**](input-elements.md#input.text) fields and an **Action.Submit** action. If both fields are filled and the button is clicked then the data sent to Looply will look like this:

```json
{
    "data": {
        "action": "APPROVE",
        "input1": "hello",
        "input2": "world"
    }
}
```

{% hint style="info" %}
Note: Looply will know which workflow this data will be sent to when a user clicks on the submit button, therefore, you may see additional information being transferred back to the Looply Workflow.
{% endhint %}

#### Technical Documentation

[**Read more about Action.Submit**](https://adaptivecards.io/explorer/Action.Submit.html)

#### [**Check out Chapter 5 of our Building Adaptive Cards tutorial!**](../../tutorials/building-adaptive-cards.md)

## Action.ShowCard

#### Description

This action will trigger a new card to appear within your current card. This is perfect if you don't want to show aspects of your card until the end user clicks on this action. A common use case for this is if a user wants to reject a card yet if they reject will have to supply a reason.&#x20;

Here's an example below:

```json
{
    "type": "AdaptiveCard",
    "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
    "body": [
        {
            "type": "Input.Text",
            "id": "input1",
            "placeholder": "Placeholder text"
        },
        {
            "type": "Input.Text",
            "id": "input2",
            "placeholder": "Placeholder text"
        }
    ],
    "version": "1.5",
    "actions": [
        {
            "style": "positive",
            "data": {
                "action": "APPROVE"
            },
            "title": "Approve",
            "type": "Action.Submit"
        },
        {
            "title": "Reject",
            "type": "Action.ShowCard",
            // nested card start
            "card": {
                "body": [
                    {
                        "isRequired": true,
                        "errorMessage": "You must select a rejection reason.",
                        "id": "choice",
                        "placeholder": "Rejection Reason",
                        "label": "Rejection Reason",
                        "choices": "${$root.payload.output.util_data}",
                        "type": "Input.ChoiceSet"
                    }
                ],
                "type": "AdaptiveCard",
                "actions": [
                    {
                        "data": {
                            "action": "REJECT"
                        },
                        "title": "Send",
                        "type": "Action.Submit"
                    }
                ],
                "version": "1.5"
            }
            // nested card end
        }
    ]
}
```

The above is a card with a "Reject" button that if clicked will show an [**Input.ChoiceSet**](input-elements.md#input.choiceset) where they will have to select a value to send back to Looply in order to reject the card.

{% hint style="info" %}
Note: Data from inputs in this nested card will still be sent back to Looply as there is an [**Action.Submit**](actions.md#action.submit) button connected. Data sent to Looply will follow the same principles as shown in the [**Action.Submit**](actions.md#action.submit) section.
{% endhint %}

#### Technical Documentation

[**Read more about Action.ShowCard**](https://adaptivecards.io/explorer/Action.ShowCard.html)

## Action.ToggleVisiblity

#### Description

This is a simple action that will show and hide select elements within the adaptive card.

#### Technical Documentation

[**Read more about Action.ToggleVisibility**](https://adaptivecards.io/explorer/Action.ToggleVisibility.html)

## Action.Execute

#### Description

This action provides no use within Looply and should not be used. This action is useful if the card is implemented on an independent system from Microsoft Teams where a developer will independently make an invoke call to a Microsoft Bot/App. This button is identical in design to the [**Action.Submit**](actions.md#action.submit) action yet the data is not automatically sent anywhere as this process is independently implemented by the hosting app.

#### Technical Documentation

[**Read more about Action.Execute**](https://adaptivecards.io/explorer/Action.Execute.html)
