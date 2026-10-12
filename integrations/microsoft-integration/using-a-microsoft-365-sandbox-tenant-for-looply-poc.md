# Using a Microsoft 365 Sandbox Tenant for Looply POC

During Proof of Concept (POC) phases, some organizations may prefer not to connect their enterprise Microsoft Azure Active Directory (Azure AD) or Microsoft Teams tenant to Looply’s public environment.

To accommodate this, Looply supports integration with a sandbox Microsoft 365 tenant—allowing end-to-end testing of approvals, notifications, and user interactions in Microsoft Teams without touching production systems.

This option helps IT and Cybersecurity teams safely validate the integration architecture while maintaining full isolation from live corporate data and users.

***

### Why Use a Sandbox Tenant

Connecting Looply to a sandbox Microsoft 365 tenant provides:

* A safe environment for testing Teams-based adaptive cards and workflow approvals
* No dependency on the enterprise tenant’s IT/Cyber approvals during POC
* Freedom to simulate user interactions and Teams notifications using fictitious accounts
* A clear migration path to the production tenant after internal review
* All data exchanged remains within Looply’s public instance and the sandbox Microsoft 365 environment.
* No production Azure AD or corporate identities are involved.

### Implementation Steps

| Step                                   | Description                                                                                                                                                                                                                                                                                                |
| -------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1. Create a Sandbox Tenant             | Sign up for a Microsoft 365 E5 Developer Subscription through the [Microsoft 365 Developer Program](https://learn.microsoft.com/en-us/office/developer-program/microsoft-365-developer-program-get-started). This provides 25 user licenses, Azure AD directory, and a full Teams environment for testing. |
| 2. Configure Azure AD & Teams          | Register the Looply app within the sandbox Azure AD tenant and grant consent for Microsoft Graph API permissions (Teams activity, profile, email, etc.). Install the Looply Teams app within this sandbox environment.                                                                                     |
| 3. Simulate Users & Workflows          | Create fictitious Teams user accounts (e.g., approver1@contoso.onmicrosoft.com) to test adaptive card notifications, approvals, and responses end-to-end.                                                                                                                                                  |
| 4. Maintain Isolation                  | The sandbox tenant remains fully isolated—no integration with production Azure AD or corporate identity systems is required. All activity and testing data remain confined to the sandbox.                                                                                                                 |
| 5. Transition to Production (Optional) | Once the POC is complete, the same configuration and app registration can be migrated to the enterprise tenant, following standard IT and Cyber governance approvals.                                                                                                                                      |

### Security & Compliance Alignment

* Looply authenticates against the sandbox tenant using the same OAuth 2.0 / SAML 2.0 flows as it would in production.
* The sandbox tenant provides full Azure AD identity control for realistic validation without production risk.
* All traffic remains encrypted (HTTPS / TLS 1.2+).

### References

* [Microsoft 365 E5 Developer Program – Get Started](https://learn.microsoft.com/en-us/office/developer-program/microsoft-365-developer-program-get-started)![Attachment.tiff](file:///Attachment.tiff)
* [Looply Security and Identity Documentation](https://academy.looply.ai/security-and-identity-what-it-teams-need-to-know)![Attachment.tiff](file:///Attachment.tiff)
