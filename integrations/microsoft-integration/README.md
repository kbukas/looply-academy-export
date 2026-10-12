---
description: Connecting Looply with Microsoft Teams (Azure AD Integration)
---

# Microsoft Integration

Integrating Looply with your organization’s Microsoft 365 and Azure Active Directory (Azure AD) environment unlocks a secure, enterprise-grade connection between Looply and Microsoft Teams.

This connection enables your organization to seamlessly deploy Looply apps, deliver adaptive card notifications to employees, and manage authentication through Microsoft’s trusted identity platform — all with centralized control and auditability.

***

### Benefits of Connecting Looply to Microsoft

| Benefit                      | Description                                                                                                                                                                                      |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Seamless App Deployment      | Publish Looply-designed apps directly to your organization’s Teams App Catalog, making them instantly available to your workforce.                                                               |
| Centralized Management       | Admins can remotely install, update, and remove Looply apps across the organization while maintaining full visibility into usage and installations.                                              |
| Adaptive Card Notifications  | Send interactive Adaptive Cards — such as approval requests, alerts, or surveys — directly to Microsoft Teams users by their corporate email or Azure AD identity.                               |
| Single Sign-On (SSO)         | Users can sign into Looply apps using their existing Microsoft credentials, ensuring secure, frictionless authentication with Azure AD policies (e.g., MFA, Conditional Access).                 |
| Enterprise-Level Security    | Integration leverages Microsoft Graph APIs within consented permissions. All activity is logged and authorized by your organization’s Azure AD, ensuring compliance with corporate IT standards. |
| Improved Employee Engagement | Approvals, surveys, and notifications arrive directly where employees already collaborate — inside Teams — increasing response rates and reducing turnaround time.                               |

***

### How the Integration Works

Once your Microsoft tenant is connected, Looply communicates with Teams through secure Microsoft Graph API calls authorized by your organization’s Azure AD.

```
Looply Platform  ↔  Azure Active Directory  ↔  Microsoft Teams
      |                                          |
      |---- Authentication & Permissions --------|
      |---- App Deployment & Adaptive Cards -----|
```

All permissions are granted through organizational consent by an Azure AD administrator, and every interaction between Looply and Microsoft services is authenticated and auditable.

***

### Connecting Microsoft to Looply

[Only an Azure AD Global Administrator or Privileged Role Administrator can authorize this connection.](#user-content-fn-1)[^1]

#### Steps to Connect

1. Log in to the Looply Dashboard.
2. Go to Settings → Integrations → Microsoft.
3. Click “Connect” on the Microsoft integration card.
4. Sign in using your organization’s Microsoft admin account.
5. Review and approve the permission scopes displayed.
6. Once consent is granted, you’ll be redirected back to Looply, where your Microsoft tenant connection details will appear.

> The integration requires organizational consent to specific Microsoft Graph API permissions that enable app deployment, adaptive card delivery, and directory lookups. Looply does not request or store any data outside these approved scopes.

***

### Post-Connection Capabilities

After connecting, your organization can:

* Deploy and update Looply apps directly from the Teams admin center.
* Send adaptive cards to internal users and groups.
* Manage installations and monitor app usage within the Looply Integration Dashboard.
* Enable Microsoft-based authentication for Looply workflows and user actions.

***

### Further Reading

* [Security and Identity – What IT Teams Need to Know](../../security-and-identity-what-it-teams-need-to-know.md)
* [Using a Microsoft 365 Sandbox Tenant for Looply POC](using-a-microsoft-365-sandbox-tenant-for-looply-poc.md)
* [Authenticating Teams User Actions to Enterprise Systems](../../authenticating-teams-user-actions-to-enterprise-systems.md)

***

[^1]: 
