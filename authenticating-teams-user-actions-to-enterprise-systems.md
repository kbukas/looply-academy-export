# Authenticating Teams User Actions to Enterprise Systems

## How Looply Connects Microsoft Teams Users to Enterprise Systems

Looply enables Microsoft Teams users to interact securely and seamlessly with enterprise workflows such as SAP approvals, without requiring separate logins, emails, or portal navigation.

This page explains how user identity is validated and securely propagated between Microsoft Teams, Looply, and enterprise backends.

***

### User Identity Mapping

When a notification is sent:

* Looply uses the user’s organizational email address to deliver the notification securely inside Microsoft Teams.
* All notification delivery is tied to the authenticated Microsoft Teams user session, ensuring the correct user receives and interacts with the workflow.

***

### Authenticating Users Back to Enterprise Systems

When a user acts on a notification (e.g., approve, reject, submit):

* Looply validates the user’s identity before committing any updates to the enterprise system.
* Authentication models vary based on the backend system’s capabilities.

***

### Supported Authentication Methods for SAP Integration

#### Basic Authentication

* SAP dialog user credentials are securely passed during the request.
* Suitable for simple SAP environments.

***

#### OAuth 2.0 Authorization Code Grant

* Users are securely redirected to SAP’s OAuth login page inside Microsoft Teams.
* SAP collects credentials and issues an authorization code, which Looply exchanges for an access token.
* The access token is then used to perform secure OData operations (e.g., POST, UPDATE) on SAP.

***

#### SAML 2.0 Bearer Assertion (OBO Flow)

* Looply leverages Microsoft Teams user session (authenticated with Azure AD).
* A SAML assertion is obtained from Azure AD and exchanged for an SAP OAuth token.
* As long as the Microsoft email matches the SAP SU01 email, Looply seamlessly impersonates the user in SAP.
* Enables true Single Sign-On (SSO) for SAP workflow approvals in Teams.

***

> 📌 The SAML2 OBO flow provides the most seamless and secure authentication experience, ensuring that users approve workflows without additional logins.

***

### Extending This Model Beyond SAP

Looply’s authentication framework is designed to be flexible.

It can be adapted to other enterprise systems that support OAuth 2.0 standards.

***

### Summary

* All user actions are validated securely before committing to enterprise systems.
* Only authorized users with matching identities can act on workflows.
* Authentication methods are configurable based on the customer’s backend system architecture.
