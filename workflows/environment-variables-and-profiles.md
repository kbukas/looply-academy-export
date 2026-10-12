---
description: >-
  Define workflow environment-specific variables to keep your logic clean,
  flexible, and reusable.
---

# Environment Variables & Profiles

Workflow **Environment Variables** in Looply allow you to define reusable sets of values that can be injected into your workflow at runtime. Each environment consists of a **profile -** a JSON object containing key-value pairs - that can be used to dynamically configure workflow behaviour. You can create multiple profiles per workflow, select which one to use at runtime, and set a **default profile** to fall back on when no specific selection is made. This helps keep your workflows clean, flexible, and easy to maintain across different environments or use cases.

## Managing & Using Environment Variable Profiles

Environment variables in Looply make it easy to configure workflow behaviour dynamically - without hardcoding values into each step. With environment profiles, you can switch between sets of values tailored for different environments like `dev`, `qa`, `staging`, or `prod`.

<figure><img src="../.gitbook/assets/Screenshot 2025-03-25 at 09.27.20.png" alt=""><figcaption></figcaption></figure>

### Creating & Managing Profiles

To manage environment variable profiles:

1. Open your workflow in the **Workflow Studio**.
2. Click the **workflow settings menu (⋮)** and select **Environment Variables**.
3. In this section, you can:
   * **Create** new profiles for different environments.
   * **Edit** each profile’s JSON data to suit the needs of that environment.
   * **Set a default profile**, which is used when no other profile is specified.
   * **Rename** existing profiles.
   * **Duplicate** profiles
   * **Delete** profiles that are no longer needed.

Each profile is essentially a JSON object of key-value pairs that will be injected into the workflow’s payload at runtime.

### Selecting a Profile for Execution

<figure><img src="../.gitbook/assets/Screenshot 2025-03-25 at 09.27.49.png" alt=""><figcaption></figcaption></figure>

Once profiles are set up, executing a workflow with a specific profile is easy:

1. Click **Execute** in the top-right of your workflow.
2. A dialog will appear prompting you to select an **Execution Profile** from a dropdown.
3. Choose the profile you want (e.g. `prod`, `staging`, `dev`) and click **Execute**.

The selected profile’s environment variable data will be injected into the payload and accessible to all steps in the workflow.

{% hint style="info" %}
💡 **Tip:** You can also reference environment variables as parameters in function steps, conditions, or integrations using the workflow data binding menu.
{% endhint %}

#### Developer API

For advanced usage or automated execution, environment variable profiles can also be selected programmatically via the **Developer API**. See the Developer API documentation for details on how to pass a profile name as part of your workflow execution payload.

{% content-ref url="../api-reference/workflow-api.md" %}
[workflow-api.md](../api-reference/workflow-api.md)
{% endcontent-ref %}
