---
description: Workflow Monitoring and Management Made Simple
---

# Monitoring Workflows

**Looply provides powerful tracking and monitoring for all your workflows.**\
Instantly view the real-time status of your workflows — whether they are in progress, completed, awaiting response, or failed — and gain detailed insights into each step of the execution.

## Workflow History

The Workflow History page provides a full overview of all your executed Looply Workflows — whether they are in-progress, completed, awaiting response, or failed.

<figure><img src="../.gitbook/assets/Screenshot 2025-04-28 at 12.48.31.png" alt=""><figcaption></figcaption></figure>

Each workflow execution is listed with the following information:

* **Date and time** the execution was triggered
* **Process ID** (unique identifier for the execution or external process)
* **Workflow ID**
* **Workflow Name**
* **Version Number** of the Workflow
* **Trigger Type** (e.g., HTTP request, Scheduled Workflow)
* **Current Status** (such as In Progress, Awaiting Response, Completed, or Failed)
* **Warnings**

### Search and Filtering

Finding specific workflow executions is easy with Looply’s powerful search and filtering options.

#### Global Search

Use the global search bar to search across:

* **Process ID**
* **Workflow ID**
* **Workflow Name**

<figure><img src="../.gitbook/assets/Screenshot 2025-04-28 at 12.50.11.png" alt=""><figcaption></figcaption></figure>

You can add **one or more search terms** to refine your results:

* Enter a search term and either **click the search icon** or **press Enter** to add it.
* Search terms will appear as **tags** above the Workflow History table.
* Remove a search term at any time by clicking the **X icon** beside its tag.
* **Wildcard searching** is supported by using the `*` character within a search term (e.g., `HOL*` will match anything beginning with `HOL`).

When multiple search terms are entered, any record that matches **any of the search terms** will be returned.

#### Filters

<figure><img src="../.gitbook/assets/Screenshot 2025-04-28 at 12.48.42.png" alt=""><figcaption></figcaption></figure>

Further refine your search results using filters:

* **Status**: Filter workflow executions by one or more execution statuses (e.g., Failed, Awaiting Response).
* **Sort Order**: Sort executions by **Newest First** or **Oldest First**.
* **Date Range**: Apply a custom date range to show executions triggered within specific dates.

#### Pagination

Effortlessly browse through your filtered results using enhanced pagination controls, ensuring smooth navigation across multiple pages of execution data.

## Examining Executions

To view detailed information about a specific workflow execution, navigate to the Workflow History table and **click on the Process ID** of the execution you want to inspect.\
This will open the **Workflow Execution Detail Page**, a fully interactive screen that displays the execution's full journey step-by-step.

<figure><img src="../.gitbook/assets/Screenshot 2025-04-28 at 12.54.00.png" alt=""><figcaption></figcaption></figure>

Within this view, each workflow step is visualized, allowing you to:

* See the overall flow and structure of the execution.
* Click on any workflow step to review its inputs, outputs, timing, and success status.

### Step Details

Selecting a workflow step within the execution provides an in-depth view of:

* **Input parameters** configured for the step.
* **Start and completion timestamps** for the step.
* **Total duration** the step took to execute.
* **Current status** of the step (Success, Failed, Awaiting Response, etc.).

Refer to the [Step Status Table](monitoring-workflows.md#step-statuses) for definitions of all possible step statuses.

### Step Input JSON

Each step within a workflow requires specific inputs to function correctly.\
Inputs can be:

* **Manually entered** during workflow design, or
* **Bound dynamically** from the payload or outputs of previous steps.

When examining a step during execution, Looply displays the **Input JSON**, which includes:

* **Current Step Inputs**: All fields specific to the selected step.
* **Previous Step Outputs**: Nested under the `$` attribute.

**Example Input JSON (Dispatch Adaptive Card Step):**

```json
{
  "allow_responses": true,
  "card_version": 1,
  "previous_card_version": 0,
  "accessible_nodes": ["****"],
  "card_id": "****",
  "previous_card_id": "",
  "card_name": "Card Name 1",
  "name": "****",
  "recipient": "",
  "action": "SEND",
  "state_id": "adaptiveCard_1",
  "app_id": "****",
  "value": "",
  "$": {
    "payload": {
      "description": "Request",
      "output": {}
    },
    "function_3": {
      "description": "Time Formatter Function",
      "output": "2023-10-18"
    }
  }
}
```

🔹 The **top level** contains the current steps's specific inputs.\
🔹 The **`$` attribute** contains the accumulated outputs from all previous steps.

{% hint style="info" %}
**Note:** The `payload` section inside `$` contains the original data passed to the workflow, visible primarily in workflows triggered via HTTP requests.
{% endhint %}

### Step Output JSON

The Output JSON shows the data generated by the currently selected step.

**Example Output JSON (Function Step):**

```json
{
  "payload": {
    "description": "Request",
    "output": {}
  },
  "function_3": {
    "description": "Time Formatter Function",
    "output": "2023-10-18"
  }
}
```

🔹 Unlike Input JSON, **previous step outputs are listed at the top level** (not nested under `$`).\
🔹 However, this output will become part of the `$` attribute in the Input JSON for subsequent steps.

### Error Logging

If a step fails due to incorrect configuration or unexpected issues, Looply provides detailed error outputs.\
These include the **error type**, **error message**, and a **stack trace**.

**Example Error Output (Dispatch Adaptive Card Step):**

```json
{
  "errorType": "Error",
  "errorMessage": "No user found",
  "trace": [
    "Error: No user found",
    "    at Runtime.dispatchAdaptiveCard [as handler] (/var/task/src/handlers/step-function/dispatchAdaptiveCard.js:183262:13)",
    "    at processTicksAndRejections (node:internal/process/task_queues:96:5)"
  ]
}
```

🔹 Errors are displayed directly within the step details, enabling you to quickly diagnose and correct issues.

**Want to stay instantly informed when a workflow fails?**\
Learn how to configure [**Error Notifications** ](error-notifications.md)to automatically alert you when issues occur.

### Step Statuses

| Status             | Description                                                                                                 | Status Colour |
| ------------------ | ----------------------------------------------------------------------------------------------------------- | ------------- |
| SUCCESS            | Step successfully completed without any errors and has passed its result onto the next step                 | Green         |
| FAILED             | Step has errored out. Error will be shown against the steps Output JSON                                     | Red           |
| TERMINATED         | Step execution has been halted and will not continue                                                        | Red           |
| IN\_PROGRESS       | This step is in progress and is yet to complete                                                             | Orange        |
| AWAITING\_RESPONSE | The Looply Workflow has stopped and is waiting from a response from external resume or Adaptive Card Action | Orange        |

### Terminating Executions

Looply gives users full control over their workflows, including the ability to **terminate executions** that are either:

* In Progress, or
* Awaiting Response.

Terminating an execution is useful when:

* A process becomes irrelevant.
* An urgent correction is needed.
* An error requires immediate stopping.

<figure><img src="../.gitbook/assets/Screenshot 2025-04-28 at 13.01.25.png" alt=""><figcaption></figcaption></figure>

To terminate a workflow:

1. Open the **Workflow Execution Detail Page** for the execution.
2. Select **Terminate**.
3. The workflow will halt immediately, and no further steps will execute.

This ensures you maintain control, protect operational integrity, and avoid letting incomplete or erroneous workflows continue.
