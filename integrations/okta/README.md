---
description: Integrating Looply with Okta using OAuth 2.0
---

# Okta

This guide explains how to integrate Looply with Okta using the OAuth 2.0 Authorization Code grant.

It is intended for customers, partners, and IAM teams configuring Single Sign-On (SSO) for Looply.

The document focuses only on what Looply requires to perform OAuth-based authentication. Okta-specific implementation details may vary by organisation.

***

## Overview

Looply integrates with Okta using a user-delegated OAuth 2.0 flow.

This allows Looply to securely obtain tokens from Okta and call downstream systems (for example, SAP BTP) on behalf of the authenticated user.

Key characteristics:

* Looply does not manage user credentials
* Okta remains the identity provider
* Tokens are issued per user
* Authentication typically runs inside a Microsoft Teams popup (this does not affect OAuth configuration)

***

### Authentication Model

| **Aspect**            | **Value**                         |
| --------------------- | --------------------------------- |
| Authentication type   | User-delegated                    |
| Protocol              | OAuth 2.0                         |
| Grant type            | Authorization Code                |
| Tokens used by Looply | Access Token, Refresh Token       |
| ID Token              | Optional (not required by Looply) |

***

### OAuth 2.0 Flow Used by Looply

Looply uses the standard OAuth 2.0 Authorization Code flow:

1. User is redirected to Okta for authentication
2. Okta authenticates the user (or reuses an existing session)
3. Okta redirects back to Looply with an authorization code
4. Looply exchanges the code for tokens
5. Looply uses the Access Token to call downstream APIs
6. Looply uses the Refresh Token to renew access tokens when required

***

### Information Required to Configure Okta

#### 1. OAuth Client Configuration

An OAuth client must be created in Okta for Looply.

| **Setting**   | **Requirement**    |
| ------------- | ------------------ |
| Client type   | OAuth 2.0 client   |
| Grant type    | Authorization Code |
| Refresh token | Enabled            |
| Client secret | Required           |

***

#### 2. Redirect URI (Required)

Okta must be configured with the following redirect URI:

```
https://passport.looply.ai/custom
```

This is where Okta sends the authorization code after successful authentication.

***

#### 3. OAuth Endpoints

Looply needs access to the following Okta endpoints:

| **Endpoint**           | Purpose                                         |
| ---------------------- | ----------------------------------------------- |
| Authorization endpoint | Initiates user authentication                   |
| Token endpoint         | Exchanges authorization code and refresh tokens |
| JWKS endpoint          | Validates JWT signatures                        |

These endpoints are usually available via Okta’s discovery configuration.

***

#### 4. Scopes Required

Looply requires the following scopes:

| **Scope**       | **Purpose**                      |
| --------------- | -------------------------------- |
| openid          | Standard OAuth/OpenID scope      |
| profile         | Basic user attributes            |
| email           | Stable user identifier           |
| offline\_access | Required to issue refresh tokens |

***

#### 5. Token Requirements

| **Token**     | **Required** | **Description**                  |
| ------------- | ------------ | -------------------------------- |
| Access Token  | Yes          | JWT used to call downstream APIs |
| Refresh Token | Yes          | Used to obtain new access tokens |
| ID Token      | Optional     | Not required by Looply           |

***

#### 6. Claims Required in the Access Token

Looply relies only on standard JWT claims.

| **Claim** | **Purpose**                   |
| --------- | ----------------------------- |
| sub       | Stable unique user identifier |
| email     | Used for user correlation     |
| iss       | Token issuer                  |
| aud       | Token audience                |

No custom claims or mappings are required.

***

#### 7. User Assignment

The OAuth client should be assigned to:

* A specific Okta group, or
* A defined set of users

This controls who is allowed to authenticate into Looply.

***

### Required Configuration Details

The following details must be shared with the Looply team.

| **Item**                 | **Description**                                                       |
| ------------------------ | --------------------------------------------------------------------- |
| Client ID                | OAuth client identifier issued by Okta                                |
| Client Secret            | Secret used by Looply to exchange the authorization code              |
| Authorization Endpoint   | Okta endpoint used to initiate user authentication                    |
| Token Endpoint           | Okta endpoint used to exchange authorization codes and refresh tokens |
| JWKS Endpoint (Optional) | Okta endpoint used to validate JWT signatures                         |
