# Signing Up & Onboarding Your Team

Getting started with Looply is a simple and secure process. This page walks you through setting up your Looply organization, inviting team members, and preparing your environment for integration.

***

### 1. Signing Up for a Looply Account

To begin, a designated team member (typically an IT admin, project lead, or business sponsor) should create the organization's Looply account.

* Visit: [https://dashboard.looply.ai/sign-up](https://dashboard.looply.ai/sign-up)
* Sign up using your enterprise email address (Microsoft Entra ID / Azure AD based email is preferred)

> 📌 **Important:**\
> The first user who signs up becomes the **Organization Owner (Super Admin)** by default. This user will manage key administration activities, including inviting other users, connecting Azure tenants, and managing integrations.

***

### 2. Inviting Team Members

After signing in:

1. Navigate to **Organisation → Team Manager** from the Looply admin cockpit sidebar
2. Click **Add Team Member**
3. Enter the email address of the colleague you want to invite
4. Assign a role to each user:

| Role          | Permissions                                                                                                                                      |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Admin**     | Full administrative access, including the ability to connect Azure tenant, deploy Teams app, manage users, and configure workflows               |
| **Developer** | Access to build and manage workflows, adaptive cards, and test integrations. No access to Azure tenant connection or organization-wide settings. |

5. Click **Send Invite** to complete the invitation process

> 📌 Admins can later modify user roles or deactivate accounts if responsibilities change.

***

### 3. Setting Up Azure Tenant Connection

To allow Looply to integrate with your Microsoft Teams environment, an Admin must connect your Microsoft Azure AD tenant.

Steps:

1. In the Looply admin cockpit, navigate to **Workflow → Integration**
2. Under **Microsoft Teams**, click **Connect**
3. Sign in using a **Global Administrator** or **Privileged Role Administrator** Azure AD account
4. Review and approve the Microsoft Graph API permission request
5. Select **Consent on behalf of your organization** and click **Accept**

This step enables:

* Deployment of the Looply app across your Microsoft Teams environment
* User directory lookup for workflow routing
* Management of app updates and permissions centrally

***

### 4. Preparing Your SAP Environment _(Only for SAP Integration Customers)_

If you plan to integrate Looply with SAP workflows:

* Install the Looply ABAP Add-on on your SAP Gateway system
* Configure an OAuth 2.0 client for secure delegated access
* Ensure relevant OData services are accessible externally or proxied securely
* Match SAP user emails (SU01) with Azure AD emails for identity propagation

👉 Full details are available in the [SAP Integration: ABAP Add-on & Access](https://academy.looply.ai/getting-started/sap-integration-abap-addon-access) section.

***

### 5. Best Practices During Onboarding

* Assign at least **two Admins** for redundancy
* Use **enterprise-managed accounts** (no personal emails)
* Coordinate early with your **SAP Basis team** if SAP integration is planned
* Validate outbound HTTPS access early to avoid network issues later

***

### Need Help?

If you encounter any issues or have questions during onboarding, please contact [support@looply.ai](mailto:support@looply.ai).

Our solutions team can also schedule a 30-minute onboarding workshop to help your team accelerate deployment.

***

✅ Once onboarding is complete, you're ready to start building workflows, dispatching intelligent notifications, and modernizing your enterprise approvals!
