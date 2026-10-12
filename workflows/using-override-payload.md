---
description: >-
  In the intricate ecosystem of workflow automation, the agility to adapt and
  modify data as it flows from one step to another is a cornerstone of dynamic
  process management
---

# Using Override Payload

The **Override Payload** step offers users the ability to either replace or update the current workflow payload with new or modified data. This capability ensures that subsequent steps in the workflow can operate on the most current and relevant data, enabling a level of customisation and responsiveness that is critical in today’s fast-paced environments.

## Configuring Override Payload

You can get started configuring an override payload step by clicking on a **+** button in your workflow and selecting the **Override Payload** step from the Workflow's toolbox. &#x20;

This step will automatically pull in your most up to date payload data from your workflow schema and auto-complete the payload editor with it.&#x20;

### Using Parameters

To make changing your payload data easier, you can configure a set of parameters that can hold either a static value or dynamic value bound from your workflow steps.&#x20;

Configure your parameters by adding a name and value for each parameter, tailoring the data to your specific needs. Need more parameters? No problem. Click the **+** **Add parameter** button to add more.

Users can decide to manually enter static values for each parameter, or use the dropdown menu to select a value from a previous step in your workflow.&#x20;

### Payload Editor

To help you get started, the  JSON payload editor will be loaded automatically with your most up to date payload data pre-filled.&#x20;

If your payload data changes while you're working on this step, you can update the editor with the latest changes by simply clicking the **Restore** button. &#x20;

{% hint style="info" %}
**Note:** Clicking **Restore** within the payload editor will pull in your latest payload data from your workflow schema - but any changes you have made within the payload editor may be lost.&#x20;
{% endhint %}

You can access the JSON payload editor to add new values or simply update the existing ones. Values can be assigned by entering them manually or referencing any previously configured parameters.&#x20;

Parameters can be accessed in your editor by referencing the parameter in the following format: `${parameter_name}`

### Testing Changes

You can test your updated payload at any time by clicking the **Test Step** button located in the header of the Override Payload sidebar.&#x20;

The output of your new payload will be returned and displayed reflecting any new attributes, values or bound data.&#x20;

{% hint style="danger" %}
**Note:** Override Payload is listed as an Advanced step within Looply Workflows. Updating or removing payload values of significant importance within your workflow steps may cause breaking changes.&#x20;
{% endhint %}
