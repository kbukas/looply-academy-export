---
hidden: true
---

# Bulk Rollout

## Deploying Looply to Your Organisation via Microsoft Teams

This guide walks your IT administrator through deploying the Looply app to users across your organisation using Microsoft Teams Admin Center and Azure Active Directory (Entra ID).

> **Heads up — bulk rollout takes time** After completing the steps in this guide, the Looply app will not appear for users immediately. Microsoft Teams processes policy assignments in batches across your tenant. Depending on the size of your organisation, this can take anywhere from **a few minutes to 24 hours**. This is expected behaviour controlled by Microsoft — it is not an issue with Looply. Please plan your rollout accordingly.

***

### Before You Begin

Make sure you have the following before starting:

* **Global Administrator** or **Teams Administrator** role in Microsoft 365
* Access to [Azure Portal](https://portal.azure.com) and [Teams Admin Center](https://admin.teams.microsoft.com)
* Looply already consented and configured in your tenant — your Looply account manager will confirm this

> Not sure if Looply is configured for your tenant? Contact your Looply account manager before proceeding. Deploying the app before tenant setup is complete will result in users seeing an error when they open it.

***

### Choosing Your Rollout Approach

Before creating groups or policies, decide how broadly you want to deploy Looply. There are three common approaches:

#### Organisation-wide

Every user in the organisation gets Looply deployed. This suits companies where Looply is a core tool used across all departments — for example, a business that has automated company-wide processes such as onboarding, IT requests, or expense approvals.

* Create a single security group with a dynamic rule like `user.accountEnabled -eq true` to capture all active users
* Or assign the policy directly to all users if your tenant is small
* Any new hire added to Azure AD will automatically receive Looply without any admin action

#### Team or department

Looply is deployed to specific teams — for example, Finance, Purchasing, HR, or Operations. This is the most common approach for organisations that are rolling out Looply incrementally or where only certain departments use automated workflows.

* Create one security group per team: `Looply - Finance Team`, `Looply - Purchasing Team`, `Looply - HR Team`
* Use dynamic membership rules based on the `department` attribute so the groups stay in sync automatically as staff join or move teams
* All groups are assigned to the same `Looply App Policy` — you do not need a separate policy per team
* Example: The Finance team uses Looply for invoice approval workflows. Only Finance staff need the app, so only the `Looply - Finance Team` group is created and assigned

#### Process-specific

Looply is deployed only to users involved in a specific business process, regardless of their department. This suits scenarios where a workflow spans multiple teams but only involves a subset of users.

* Example: An invoice approval process involves the Purchasing team, Finance approvers, and a small group of senior managers. Rather than deploying to entire departments, you create a single group — `Looply - Invoice Approvers` — and add only the relevant users
* Membership is typically managed manually (Assigned type) since the group is curated rather than department-driven
* This approach gives the tightest control over who has access

> **Tip — You can combine approaches** Many organisations start with a team-based rollout (e.g. Finance first) and expand over time. Each team gets its own security group, all assigned to the same Teams Setup Policy. Process-specific groups can be added later for cross-departmental workflows without disrupting the existing setup.

***

### Step 1 — Create a Security Group in Azure Portal

You will use a security group to control which users receive the Looply app.

1. Go to [portal.azure.com](https://portal.azure.com) and sign in with your admin account
2. Navigate to **Azure Active Directory** (Microsoft Entra ID) → **Groups**
3. Click **+ New group**

Fill in the form as follows:

| Field             | Value                                                           |
| ----------------- | --------------------------------------------------------------- |
| Group type        | **Security**                                                    |
| Group name        | e.g. `Looply - Finance Team` or `Looply - Purchasing Team`      |
| Group description | e.g. `Finance team users with Looply deployed via Teams policy` |
| Membership type   | See below                                                       |

#### Membership Type — Which Should You Choose?

**Assigned** — You manually add and remove members. Best for smaller teams or process-specific groups where membership doesn't change often (e.g. a fixed group of invoice approvers).

**Dynamic User** — Members are automatically added based on user attributes in Azure AD. Best for department-level rollouts where you want the group to stay in sync with your directory automatically.

> **Tip — Dynamic membership is ideal for team-based rollouts** If your Azure AD user profiles include department attributes, you can write a rule that automatically keeps the group in sync. For example:
>
> * Finance team: `user.department -eq "Finance"`
> * Purchasing team: `user.department -eq "Purchasing"`
> * HR team: `user.department -eq "Human Resources"`
> * All UK employees: `user.country -eq "GB"`
> * All permanent staff: `user.employeeType -eq "Employee"`
>
> This means when a new Finance hire is added to Azure AD with the correct department, they automatically get Looply deployed — no manual steps required.

4. Click **Create**

***

### Step 2 — Add Members to the Group

#### If you chose Assigned membership

1. Open the group you created
2. Go to **Members** → **+ Add members**
3. Search for users by name or email and select them
4. Click **Select** to confirm

> **Tip** — You can add existing security groups (e.g. an existing `Finance Department` group) as members rather than adding individual users. This avoids duplicating membership management across multiple places.

#### If you chose Dynamic User membership

1. Open the group and go to **Dynamic membership rules**
2. Click **Edit** and use the rule builder or write a rule directly
3. Click **Validate rules** to test against specific users before saving
4. Click **Save**

> Dynamic group population is not instant. After saving a rule, Azure AD may take up to 24 hours to fully evaluate all users in large tenants. Check **Membership processing status** on the group overview page to monitor progress.

***

### Step 3 — Create a Teams App Setup Policy

A Setup Policy tells Microsoft Teams to automatically install Looply for users assigned to that policy. You only need **one policy** regardless of how many groups or rollout approaches you are using.

1. Go to [admin.teams.microsoft.com](https://admin.teams.microsoft.com)
2. Navigate to **Teams apps → Setup policies**
3. Click **+ Add**

> **Do not modify the Global (Org-wide default) policy.** Create a dedicated policy for Looply. This keeps your rollout isolated and easy to reverse if needed.

4. Name the policy — e.g. `Looply App Policy`
5. Under **Installed apps**, click **+ Add apps**
6. Search for **Looply**, click **Add**, then **Add** again to confirm
7. Click **Save**

***

### Step 4 — Assign the Policy to Your Group

The Setup policies page has two tabs — **Manage policies** (where you created the policy in Step 3) and **Group policy assignment** (where you assign it to your group). Make sure you are on the correct tab.

Each group assignment is done one at a time. If you have multiple groups (e.g. Finance, Purchasing, HR), repeat this process for each group — each one is assigned to the same `Looply App Policy`.

1. In Teams Admin Center, go to **Teams apps → Setup policies**
2. Click the **Group policy assignment** tab
3. Click **+ Add group**
4. Search for and select your security group (e.g. `Looply - Finance Team`)
5. Under **Select a policy**, choose `Looply App Policy`
6. Set the **Rank** to `1` unless you have other conflicting policies
7. Click **Apply**
8. Repeat steps 3–7 for each additional group

> **Note — Rollout approach examples**
>
> * **Organisation-wide**: Assign your single all-staff group (e.g. `Looply - All Staff`) once
> * **Team or department**: Repeat the assignment for each team group — `Looply - Finance Team`, `Looply - Purchasing Team`, `Looply - HR Team` — each assigned to `Looply App Policy`
> * **Process-specific**: Assign your curated group (e.g. `Looply - Invoice Approvers`) once

***

### Installing for Individual Users

For one-off installations — for example, a new starter, a contractor, or a user who needs access outside of their team group — use the **Looply App Manager** rather than modifying your Azure AD groups or Teams policies.

The Looply App Manager allows your Looply administrator to install and uninstall the app for individual users directly from within Looply, without requiring IT involvement.

To access it, sign in to Looply and navigate to **App Manager** from the main menu. From there you can search for users and install or uninstall the app individually.

> **When to use Looply App Manager vs Teams Admin deployment**

| Scenario                                | Use                                                        |
| --------------------------------------- | ---------------------------------------------------------- |
| Deploying to a whole team or department | Teams Admin Center + Azure AD group                        |
| New starter joining an existing group   | Add them to the Azure AD group — app deploys automatically |
| One-off user outside of a group         | Looply App Manager                                         |
| Contractor or temporary user            | Looply App Manager                                         |
| Removing a specific user's access       | Looply App Manager or remove from Azure AD group           |

***

### Step 5 — Verify the Rollout

Once the policy is assigned, Teams will begin deploying Looply to users in the group.

**To check policy assignment:**

1. In Teams Admin Center, go to **Teams apps → Setup policies**
2. Click the **Group policy assignment** tab — your group should be listed with `Looply App Policy` shown against it

**To check a specific user:**

1. Go to **Users → Manage users**
2. Find the user and open their profile
3. Check the **Policies** tab — App setup policy should show `Looply App Policy`

**To verify from the user's side:** Ask a test user to open Microsoft Teams — Looply should appear in their left sidebar or app bar once the rollout has reached them.

> **Tip — Track policy assignments via the Activity log** You can audit when a policy was assigned to a group and which admin made the change. In Teams Admin Center, go to **Users → Activity log** to see a history of policy assignment operations, including timestamps and the admin account that performed each action. This is useful for confirming the rollout was triggered and when to expect it to complete.

> **Still not seeing the app?** If users don't see Looply after completing these steps, wait up to 24 hours before investigating. Microsoft Teams processes policy rollouts gradually — this is not an issue with Looply. If the app has still not appeared after 24 hours, contact your Looply account manager with your tenant ID and the name of the policy and group you created.

***

### Important — Looply Is Notified When the User First Opens Teams

When an admin deploys Looply via a Setup Policy, the app is provisioned to the user's account in the Microsoft 365 backend. However, **Looply is only notified of the installation when the user's Teams client processes it** — which happens the first time the user opens Microsoft Teams after the policy has been applied.

This means:

* If a user has not yet logged into Teams (e.g. a new starter who hasn't set up their device yet), Looply will not show them as installed until they do
* This is expected behaviour defined by Microsoft — the installation event is fired by the Teams client, not Microsoft's backend infrastructure
* There is no action required from the administrator — Looply will be notified automatically once the user opens Teams

> **Note for administrators** If you need to confirm that Looply has been successfully set up for a user before they have logged into Teams for the first time, contact your Looply account manager. Do not assume a missing user record in Looply indicates a failed installation — it may simply mean the user hasn't opened Teams yet.

***

### Removing the App from Users

| Action                 | How                                                               |
| ---------------------- | ----------------------------------------------------------------- |
| Remove a specific user | Remove them from the Azure AD group, or use Looply App Manager    |
| Remove an entire team  | Remove the group from the policy assignment in Teams Admin Center |
| Remove Looply org-wide | Delete the `Looply App Policy` entirely                           |

> Removing a user from the group or policy does **not** delete their Looply data. Their workflows and history remain intact and can be reassigned or archived by a Looply administrator.
