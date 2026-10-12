# Bulk Install (Beta)

The Bulk Install tab allows administrators to install the Looply app for multiple users at once. This is ideal for large-scale rollouts across teams, departments, or entire organisations.

{% hint style="info" %}
**Note:** Access Bulk Install from **App Manager → select your app → Bulk Install** tab.
{% endhint %}

{% hint style="warning" %}
**Prerequisite:** The app must be deployed to your organisation's Teams app catalog before bulk installation can proceed.
{% endhint %}

### Overview

Bulk Install has two views:

* **Batch Jobs** — A list of all previous and active installation jobs
* **New Batch** — A user selection screen where you build and submit a new batch

### Creating a New Batch

<details>

<summary>Step 1: Click "New Batch"</summary>

From the Batch Jobs view, click **New Batch** in the top right corner. This opens a split-screen view with a user browser on the left and your selected batch on the right.

</details>

<details>

<summary>Step 2: Select Users</summary>

The left panel displays all users in your Microsoft organisation with their **Display Name** and **User Principal Name** (email).

* Click the **checkbox** next to individual users to add them to the batch
* Use the **Select All** checkbox in the header row to select all eligible users on the current page
* Use the **search bar** to find users by name or email
* Navigate through pages to find users across your organisation

Users who already have the app installed are shown with an **Installed** badge and cannot be selected.

</details>

<details>

<summary>Step 3: Name the Batch (Optional)</summary>

Enter a name in the **Batch Name** field on the right panel. This helps identify the job later. If left blank, it appears as "Bulk Install Batch".

</details>

<details>

<summary>Step 4: Review and Submit</summary>

The right panel shows all selected users. You can:

* Review the full list before submitting
* Remove individual users by clicking the remove button next to their name
* Click **Submit** to start the installation

</details>

{% hint style="warning" %}
**Limit:** Each batch supports a maximum of **500 users**. The selection counter shows how many users are selected.
{% endhint %}

{% hint style="info" %}
**Note:** Once submitted, the batch processes in the background. You can safely navigate away — the installation will continue and you can check progress by returning to the Bulk Install tab.
{% endhint %}

### Monitoring Batch Jobs

After submitting, you are returned to the **Batch Jobs** view. Each job displays:

| Column          | Description                                    |
| --------------- | ---------------------------------------------- |
| **Batch Name**  | The name you gave the batch                    |
| **App Version** | The version of the app being installed         |
| **Status**      | Current state — see below                      |
| **Progress**    | Completed and failed installs out of the total |
| **Created**     | When the batch was submitted                   |
| **Completed**   | When the batch finished processing             |

#### Job Statuses

{% hint style="success" %}
**DONE** — All users in the batch were processed successfully.
{% endhint %}

{% hint style="warning" %}
**PARTIAL** — The batch completed but some users failed. Click the job to see details.
{% endhint %}

{% hint style="info" %}
**PROCESSING** — The batch is actively installing. The progress bar updates in real time and the list auto-refreshes every 15 seconds.
{% endhint %}

### Viewing Job Details

Click any job row to open the **Job Detail** dialog showing every user in the batch with their individual status:

{% hint style="success" %}
**Installed** — App was successfully installed. Hover for the installation timestamp.
{% endhint %}

{% hint style="info" %}
**Pending** — Still waiting to be processed.
{% endhint %}

{% hint style="warning" %}
**Failed** — Shown with a specific reason. See failure reasons below.
{% endhint %}

If there were infrastructure-level errors, they appear in a warning banner at the top of the dialog.

#### Failure Reasons

| Reason                           | Meaning                                                            |
| -------------------------------- | ------------------------------------------------------------------ |
| **Already installed**            | The app was already installed for this user                        |
| **Insufficient permissions**     | Teams admin policies do not allow installing the app for this user |
| **User not found**               | The user could not be found or resolved in Microsoft Graph         |
| **Microsoft Graph server error** | A temporary error on Microsoft's side — retried automatically      |
| **Max retries exceeded**         | Installation was retried multiple times but continued to fail      |
| **Failed to queue**              | The installation request could not be queued for processing        |

### Troubleshooting

<details>

<summary>Batch taking a long time to complete</summary>

Large batches may take several minutes due to Microsoft Graph rate limiting. The system handles retries automatically. The progress bar will continue updating — wait for the batch to complete.

</details>

<details>

<summary>Many users showing "Insufficient permissions"</summary>

Your Teams admin policies may be restricting app installation for these users. Check **Teams Admin Center → Teams apps → Permission policies** to ensure the app is allowed for the affected users or groups.

</details>

<details>

<summary>Users showing "Already installed"</summary>

No action needed — the app was previously installed for these users, either individually from the User Manager tab or in a prior batch.

</details>

<details>

<summary>Users not appearing in the user list</summary>

The user may not have an active Microsoft Teams licence. Verify in **Azure AD → Users → select the user → Licences** that a Teams-enabled licence is assigned.

</details>

<details>

<summary>Batch completed but users aren't receiving messages</summary>

The app is installed but the conversation may not be fully provisioned yet. Use the **Installation Health** tab to run health checks on affected users and identify the specific issue.

</details>

<details>

<summary>Checkboxes are disabled for some users</summary>

Either the user already has the app installed (shown with an **Installed** badge), or you have reached the **500 user limit** for a single batch. Submit the current batch first, then create a new one for the remaining users.

</details>
