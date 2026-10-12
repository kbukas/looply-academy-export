# SAP Integration: ABAP Add-on & Access

Looply connects seamlessly with SAP ECC and S/4HANA systems, enabling real-time notifications and workflow interactions inside Microsoft Teams.

This page explains the core components required to integrate Looply with your SAP landscape.

***

### Overview

To enable SAP integration, Looply requires:

* The installation of the **Looply ABAP Add-on** on your SAP Gateway system
* Configuration of OAuth 2.0 authentication in SAP Gateway
* Exposure of specific SAP endpoints externally (secure HTTPS access)

***

### Looply ABAP Add-on

The Looply ABAP Add-on is installed on your SAP system.

#### Purpose

* Exposes standardized OData services for workflow interactions
* Handles user delegation securely via OAuth 2.0 and SAML assertions
* Simplifies access management via predefined PFCG roles

#### Deployment

* Lightweight footprint; no core SAP modifications

{% hint style="info" %}
Installation instructions are detailed in the [SAP Integration Installation Guide](integrations/sap-integration/installing-the-abap-looply-add-on/).
{% endhint %}

***

### SAP OAuth 2.0 Client & Role Setup

To enable secure, delegated access between Looply and SAP:

* Create a new **OAuth 2.0 Client** in SAP Gateway
* Configure **SAML 2.0 Federation** between Azure AD and SAP
* Assign SAP users the necessary **PFCG roles** to authorize access to Looply services

Typical PFCG roles will grant controlled access to:

* OData services exposed by Looply
* Authorization token endpoints

**Note:** OAuth 2.0 setup must be completed separately for each SAP environment (DEV, QA, PROD).

***

### Making SAP Endpoints Accessible

Looply requires your SAP system to expose two key HTTPS endpoints externally:

| Endpoint                              | Purpose                          |
| ------------------------------------- | -------------------------------- |
| `/sap/opu/odata/looply/service_srv/*` | OData services for workflow      |
| `/sap/bc/sec/oauth2/token`            | OAuth 2.0 token exchange         |
| `/sap/bc/sec/oauth2/authorize`        | OAuth 2.0 authorization endpoint |

#### Options for Exposure

* **Proxy via Azure Application Gateway** (recommended)
* **Expose directly** through firewall/NAT rules&#x20;

{% hint style="success" %}
These endpoints must be accessible from Looply's hosted environment (AWS). AWS IP address for whitelisting Looply: **52.208.220.68**
{% endhint %}

If your SAP system is not externally exposed, we recommend setting up a secure proxy in Azure/applicable public facing infrastructure to enable it.

***

### Optional: SAP BTP Proxy Integration

If your organization uses SAP BTP Integration Suite and Cloud Connector, Looply can also connect to your SAP systems via BTP.

This model adds an extra layer of abstraction between Looply and your on-premise SAP Gateway.

* Useful for customers standardizing integration patterns via BTP



***
