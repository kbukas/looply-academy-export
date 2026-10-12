---
description: Redirect between steps in your Looply Workflows
---

# Using Redirects

Redirect Steps enable users to navigate within a workflow either backwards, to create a loop and repeat a series of events - or forwards, to skip over a series of steps.

## Configure Redirects

You can get started configuring your redirect by clicking on a **+** button in your workflow and selecting the **Redirect** step from the Workflow's toolbox. &#x20;

Afterward, open the **dropdown menu** in the step sidebar and choose the specific step you wish to redirect to from this point in your workflow.

#### Redirect Limitations

The following steps are **not supported** when using redirects within your workflow, and therefore cannot be selected from the step selection dropdown menu.&#x20;

* Event Trigger - prevents re-execution of entire workflow (use Trigger Workflow step instead)
* Redirect - prevents possible infinite loops or chained redirects
* Terminate - prevents redirect to termination (use direct Terminate step instead)
