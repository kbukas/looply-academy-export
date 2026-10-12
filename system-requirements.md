---
description: Minimal Requirements, Enterprise Ready
---

# System Requirements

Looply is designed to integrate seamlessly into your existing Microsoft 365 and SAP environments with minimal disruption. Our architecture avoids invasive changes, enabling fast deployment while aligning fully with enterprise security and compliance standards.

This section outlines the required prerequisites across Microsoft 365, SAP, and network layers to help your IT team quickly assess readiness.

***

### Microsoft 365 Requirements _(For Microsoft Teams Integration)_

| Requirement                                     | Details                                                                                                                   |
| ----------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| **Microsoft Entra ID (Azure Active Directory)** | Required for user authentication, delegated access, and app management                                                    |
| **Microsoft Graph API Consent**                 | Admin consent required to grant Looply access to standard scopes for Teams app management and directory lookup            |
| **Microsoft Teams Client**                      | Desktop or Web client supported; Looply custom app must be installed via Teams Admin Center or organizational App Catalog |

***

{% hint style="info" %}
If your organization prefers not to connect its production Microsoft Azure AD or Microsoft Teams tenant during the Proof of Concept (POC) phase, Looply supports a fully isolated sandbox setup.

👉 Using a [Microsoft 365 Sandbox Tenant for Looply POC](integrations/microsoft-integration/using-a-microsoft-365-sandbox-tenant-for-looply-poc.md)
{% endhint %}

### SAP System Requirements _(Only for SAP Workflow Integration)_

| Requirement                 | Details                                                                                                                       |
| --------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| **SAP NetWeaver Version**   | 750 or higher (compatible with SAP ECC and SAP S/4HANA)                                                                       |
| **Looply ABAP Add-on**      | Installation required on SAP backend system                                                                                   |
| **OData Endpoint Exposure** | `/sap/opu/odata/looply/service_srv/*` and `/sap/bc/sec/oauth2/token` must be externally accessible to Looply's cloud services |
| **OAuth 2.0 Client Setup**  | Required in SAP Gateway to enable secure, token-based user delegation                                                         |
| **Network Accessibility**   | Outbound HTTPS access from SAP Gateway is required; static IP allowlisting supported if needed                                |

***

### Network and Security Requirements

| Requirement                    | Details                                                                                                                            |
| ------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------- |
| **Outbound HTTPS Access**      | Required from SAP Gateway to Looply's public API endpoints                                                                         |
| **TLS Encryption**             | All communication encrypted using TLS 1.2+ standards                                                                               |
| **IP Allowlisting (Optional)** | Static outbound IP allowlisting available for customers enforcing strict egress policies                                           |
| **Authentication Standards**   | OAuth 2.0 delegated flows supported, including Basic Auth, OAuth Code Grant, and SAML2 Bearer Assertion (OBO) for SAP integrations |

***

If you have any questions about system compatibility or would like assistance with planning your integration, please reach out to support@looply.ai.
