---
description: >-
  Adaptive Cards are a platform-agnostic and customisable way to create
  interactive and data driven messages that are actioned upon using Microsoft
  Teams.
---

# Building Adaptive Cards

Start by clicking on the **Template Designer** and then clicking on **New Card** within the **Start menu.** Enter a name for your card and then click the **Create** Button.

## The Adaptive Card Designer

The Adaptive Card Designer, created by Microsoft, has been imported into Looply to create Adaptive Cards for Looply Workflows. It provides an easy way to create beautiful data-rich messages that can be sent to teams. This designer provides massive benefits when combined with Looply as data fetched and generated via Looply Workflows can be bound and displayed to your end users in a dynamic way. The Adaptive Card Designer is here to provide you with the tools to create a template for your Adaptive Card so that in each process a dynamically data-driven card can be created.

[**Check out our in-depth tutorial on Building Adaptive Cards**](../../tutorials/building-adaptive-cards.md)[**!**](../../tutorials/building-adaptive-cards.md)

There are 8 main areas of the Adaptive Card Designer:

### Card Toolbar

This toolbar is located at the top of the Adaptive Card Designer. This acts as a file menu where you can create new cards, undo and redo actions, deploy the card, and alter the several views of the Adaptive Card Designer. You also have quick access to an AI Assistant, and the ability to test your card to see how it would look on Microsoft Teams.

### Card Elements Toolbar

Located to the left of your screen are all the available elements ready to use to create your card. Below are links to understand each element:

* [Container Elements](container-elements.md)
* [Content Elements](content-elements.md)
* [Input Elements](input-elements.md)

### Adaptive Card Canvas

Located in the centre (just right of the [Card Elements Toolbar](./#card-elements-toolbar)) of the Adaptive Card Designer is the canvas. Drag and drop elements from the [Card Elements Toolbar](./#card-elements-toolbar) to create your card. Drag elements already placed on the canvas to realign them. Certain elements placed on the canvas have buttons to add [actions](actions.md) or more [columns](container-elements.md#columnset) if the respective element has been selected. More on this from the respective element sections from the [Card Elements Toolbar](./#card-elements-toolbar) section above.

### Card Structure Toolbar

Just right of the [Adaptive Card Canvas](./#adaptive-card-canvas), you'll find the structure of the card you are currently building. Provided as a tree structure, use this to quickly select elements.

### Element Properties Toolbar

On the right of the Adaptive Card Designer is the Element Properties Toolbar. When an element is selected on the Adaptive Card Canvas you will be provided with a menu to style the respective component. This will contain fields to change text, colour, alignment, and potentially add [actions](actions.md) if the element is clicked. Read more on this by finding out about how each element can be altered via the [Card Elements Toolbar](./#card-elements-toolbar) section above.

### Card Payload Editor

The first of the 2 text editors. This contains the JSON of the Adaptive Card that you will build. You can edit the text in this editor to add your own [bindings](../data-binding.md) or paste in an Adaptive Card you have already made elsewhere.&#x20;

{% hint style="warning" %}
Note: Manually typing in the Card Payload Editor may cause the card to disappear from the Adaptive Card Canvas. This occurs when the JSON supplied is incorrect.
{% endhint %}

### Sample Data Editor

The second of the 2 editors. This editor contains a sample data schema, which can either be hand-typed or dynamically passed in if the card was created alongside a workflow. Providing data to this text editor will provide data for the [data binding process](../data-binding.md).

### Footer Toolbar

Located at the bottom of the Adaptive Card Designer. This will alert you to the Adaptive Card's name, if it has a Looply Workflow attached, and if the Card has been [activated](./#card-versioning).

## Card Versioning

Currently, Adaptive Cards created are being versioned. Each card is initially given **Draft** status. Once the card has been **Activated** that will lock this version in place.&#x20;

{% hint style="info" %}
Note: Looply Workflows using Adaptive Cards will always use the latest **Active** Adaptive Card version.
{% endhint %}

Saving a change made to an Active Adaptive Card will trigger a new version to be made. The new version of the card that's made is automatically placed back into **Draft** status.&#x20;

{% hint style="warning" %}
Note: You can still use **Draft** Adaptive Cards within your Workflows, however, if accidental changes are made to the Card subsequent messages containing this card will be affected.
{% endhint %}

## Shortcuts

To make designing Adaptive Cards easier we created some shortcuts for frequent actions.

| Shortcut(Windows) | Shortcut(Mac) | Action       |
| ----------------- | ------------- | ------------ |
| CTRL+SHIFT+E      | CMD+SHIFT+E   | New Card     |
| CTRL+S            | CMD+S         | Save Card    |
| CTRL+SHIFT+R      | CMD+SHIFT+R   | Rename Card  |
| CTRL+Z            | CMD+Z         | Undo         |
| CTRL+SHIFT+Z      | CMD+SHIFT+Z   | Redo         |
| CTRL+SHIFT+L      | CMD+SHIFT+L   | AI Assistant |
