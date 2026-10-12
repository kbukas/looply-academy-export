---
description: >-
  In the dynamic environment of workflow automation, controlling when a workflow
  ends is just as crucial as managing its start.
---

# Terminating Workflows

The **Terminate Step** is a powerful tool designed to explicitly define a point within your workflow where the execution should immediately cease. Whether to halt a process due to an error, fulfil a specific condition, or simply end a workflow after completing its intended task, the Terminate Step provides a straightforward and effective solution.

## Adding Terminate Steps

You can get started adding a terminate step by clicking on a **+** button in your workflow and selecting the **Terminate** step from the Workflow's toolbox. &#x20;

No further configuration is required - execution will be halted immediately when this step is reached within your workflow. You can add as many terminates as you require to handle different flows within your process.

{% hint style="info" %}
Terminate Steps are append only steps - which means they cannot be inserted in between 2 steps as this would break your workflow chain.&#x20;
{% endhint %}
