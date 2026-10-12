---
description: Using Advanced Conditionals in Looply Workflows
---

# Using Advanced Conditionals

{% hint style="info" %}
New to conditionals? [Setting up advanced conditionals](https://academy.looply.ai/~/revisions/pEzhOhCQmzw0jAeIkFbI/workflows/using-advanced-conditionals/setting-up-advanced-conditionals)
{% endhint %}

The Advanced Conditional step in Looply allows users to build complex if & else logic within their workflows with multiple branches. This step enables users to create sophisticated conditional logic, ensuring that workflows can handle a wide range of scenarios and data conditions efficiently.

## **Overview**

The Advanced Conditional step is designed to offer more flexibility and power compared to the older Conditional and Branch Conditional steps. Here's how it works and how you can configure and use it:

1. **Multiple Branches**: Users can create multiple branches within an .Advanced Conditional step. Each branch can be specified as either an `IF` or `ELSE` branch, allowing for intricate logic flows.
2. **Complex Logic**: Within `IF` branches, users can build complex logic using `IF` and `AND`/`OR` conditions. This allows for detailed evaluation of data passing through the workflow from previous steps.
3. **Branch Reordering**: Users have the ability to reorder their `IF` statement branches, determining the sequence in which they are evaluated. This ensures that the most critical conditions are checked first.

<figure><img src="../../.gitbook/assets/workflow - advanced conditional example.png" alt=""><figcaption><p>Advanced Conditional - Example</p></figcaption></figure>

## Define Conditional Logic

You can get started adding your conditional logic by clicking on a **+** button in your workflow and selecting the **Advanced** **Conditional** step from the Workflow's toolbox.  &#x20;

This will add the advanced conditional block to your workflow where you can begin building your logic branches. If you insert an advanced conditional step in-between 2 existing steps, the workflow chain will be broken to allow you to build out your conditional logic and can be manually re-connected after.&#x20;

### Adding Logic Branches

You can add logic branches to your Advanced Conditional step by **clicking and dragging** from the step on to the workflow builder canvas. This will bring up the **Create Condition** dialog where you can define your condition logic.&#x20;

Advanced Conditional branches are made up of the following components:&#x20;

* **Label:** a label or description for your branch which will only be visible to you within your workflow.
* **Condition Type:** specifies the type of condition for your branch (`IF` or `ELSE`) - please note you can only have 1 `ELSE` condition per Advanced Conditional step.&#x20;
* **Rules:** the conditional rule groups that contain your `IF` statement logic - only applicable to `IF` condition types.&#x20;

<figure><img src="../../.gitbook/assets/advanced-conditional create condition.png" alt=""><figcaption><p>Advanced Conditional - Create Condition Dialog</p></figcaption></figure>

#### Adding Rule Groups

Each `IF` condition logic branch can have multiple groups of rules, with each group being defined as `AND` / `OR` logic.&#x20;

* New rule groups can be added by clicking the **+ Add Group** button.&#x20;
* Use the `AND` / `OR` icon buttons to toggle between the rule group type.
* You can use the **trash can** icon button to delete the rule group

#### Adding Rules&#x20;

Each rule group can have multiple conditions inside, all of which must be true for the overall rule group to evaluate as `true`.&#x20;

Each rule consists of an **attribute**, a **condition** and a **value**. You can use these fields to evaluate data from within your workflow.&#x20;

New rules can be added to a group by clicking the **+ Add Rule** button.&#x20;

#### Pseudocode View

<figure><img src="../../.gitbook/assets/advanced-conditional psuedocode.png" alt=""><figcaption><p>Advanced Conditional - Pseudocode View</p></figcaption></figure>

To help visualise your conditional logic, we've added an easy to understand pseudocode view which displays your conditions in plain english as you build them. &#x20;

### Re-Ordering Conditions

Once you have defined your logic branches for the Advanced Conditional step, you can use the Advanced Conditional Editor in the sidebar to specify the order your conditions should be evaluated.&#x20;

You can access the Editor by **clicking and selecting** the Advanced Conditional step from within the workflow builder canvas.&#x20;

<figure><img src="../../.gitbook/assets/advanced-conditional re-ordering conditions.png" alt=""><figcaption><p>Advanced Conditional - Re-Ordering Conditions</p></figcaption></figure>

You can re-order your conditions from within this editor by **clicking and dragging the drag icon** button (located on the left of the condition item) and moving the condition up or down within the list.&#x20;

Each branch defined here will be evaluated when your workflow from the top down.&#x20;

`ELSE` conditions **cannot be re-ordered** as they will always be executed last if no other conditions are met.&#x20;
