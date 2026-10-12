---
hidden: true
---

# Troubleshooting & FAQ

This section is organized by the stage of the authentication flow where issues typically occur.

***

### Stage 1: Okta Authentication

**Q: User sees "Unable to sign in" or gets redirected back to login**

**Symptoms:**

* User enters Okta credentials but login fails
* Browser redirects back to login page without error
* Okta shows successful login but Looply doesn't receive the token

**Possible Causes & Solutions:**

| Cause                    | How to Verify                                 | Solution                                                       |
| ------------------------ | --------------------------------------------- | -------------------------------------------------------------- |
| Redirect URI mismatch    | Check Okta app config → Sign-in redirect URIs | Ensure `https://passport.looply.ai/callback` is listed exactly |
| User not assigned to app | Okta Admin → Applications → Assignments       | Assign user or group to the Looply application                 |
| Scopes not configured    | Check Okta authorization server settings      | Ensure `openid`, `email`, `profile` scopes are allowed         |
| Token claims missing     | Decode token at jwt.io                        | Verify `email` claim is present in the ID token                |

**Diagnostic Steps:**

1. In Okta Admin Console, go to **Reports → System Log**
2. Filter by the user's email and time of login attempt
3. Look for authentication events and any error codes

***

**Q: Okta token doesn't contain the expected email claim**

**Symptoms:**

* Authentication succeeds but downstream token exchanges fail
* IAS rejects the token with "subject not found" or similar

**Solution:**

1. Go to Okta Admin → **Security → API → Authorization Servers**
2. Select your authorization server (or `default`)
3. Go to **Claims** tab
4. Verify an `email` claim exists with:
   * Include in token type: **ID Token** (or Always)
   * Value: `user.email`

***

### Stage 2: IAS Token Exchange

**Q: Error "invalid\_grant" when exchanging Okta token at IAS**

**Symptoms:**

* Looply logs show `invalid_grant` error from IAS token endpoint
* Token exchange fails at the first hop (Okta → IAS)

**Possible Causes & Solutions:**

| Cause                                     | How to Verify                                                 | Solution                                                    |
| ----------------------------------------- | ------------------------------------------------------------- | ----------------------------------------------------------- |
| Okta not configured as Corporate IdP      | IAS Admin → Identity Providers → Corporate Identity Providers | Complete Step 1.1 in setup guide                            |
| Wrong Client ID/Secret in IAS             | Compare IAS Corporate IdP config with Okta app                | Update credentials in IAS to match Okta application         |
| JWT issuer mismatch                       | Decode Okta token → check `iss` claim                         | Ensure IAS discovery URL matches the Okta issuer exactly    |
| Token expired                             | Check `exp` claim in JWT                                      | Tokens typically expire in 1 hour; ensure clocks are synced |
| Corporate IdP not enabled for application | IAS → Applications → Your App → Trust                         | Enable Okta under "Trusted Identity Providers"              |

**Diagnostic Steps:**

1. In IAS Admin Console, go to **Monitoring & Reporting → Troubleshooting Logs**
2. Enable logging for your application
3. Retry the authentication and search logs for "token" or "jwt"
4. Check the detailed error message in the log entry

***

**Q: Error "invalid\_client" from IAS**

**Symptoms:**

* IAS rejects the token exchange request immediately
* Error occurs before JWT validation

**Possible Causes & Solutions:**

| Cause                                     | Solution                                                                  |
| ----------------------------------------- | ------------------------------------------------------------------------- |
| Wrong Client ID for IAS application       | Verify Client ID in IAS → Applications → Your App → Client Authentication |
| Wrong Client Secret                       | Regenerate secret in IAS and update in Looply                             |
| Application not configured for JWT Bearer | Ensure Grant Type includes `JWT Bearer` in IAS application settings       |

***

**Q: IAS returns token but user identity is wrong or missing**

**Symptoms:**

* Token exchange succeeds but downstream services don't recognize the user
* XSUAA or SAP shows wrong user or "anonymous"

**Solution:**

1. In IAS, go to your **Corporate Identity Provider** (Okta)
2. Check **Subject Name Identifier** setting
3. Ensure it's set to `email` (or the attribute that matches your SAP users)
4. Under **Enrich Token Claims**, verify the correct claims are being passed through

***

### Stage 3: XSUAA Token Exchange

**Q: Error "invalid\_token" or "unauthorized" from XSUAA**

**Symptoms:**

* IAS token exchange succeeded, but XSUAA rejects the IAS token
* Looply logs show 401 or `invalid_token` from XSUAA endpoint

**Possible Causes & Solutions:**

| Cause                                   | How to Verify                                     | Solution                                                          |
| --------------------------------------- | ------------------------------------------------- | ----------------------------------------------------------------- |
| IAS not trusted in BTP subaccount       | BTP Cockpit → Security → Trust Configuration      | Add IAS as trusted identity provider (Step 2.1)                   |
| XSUAA instance missing jwt-bearer grant | Check xs-security.json or service instance config | Recreate instance with correct grant types                        |
| Token audience mismatch                 | Decode IAS token → check `aud` claim              | Ensure IAS application is configured to include XSUAA in audience |
| Service key credentials wrong           | Compare service key JSON with Looply config       | Update Client ID/Secret from a fresh service key                  |

**Diagnostic Steps:**

1. In BTP Cockpit, go to your subaccount → **Services → Instances**
2. Find your XSUAA instance and view the service key
3. Verify the `url`, `clientid`, and `clientsecret` match Looply's configuration

***

**Q: Error "insufficient\_scope" from XSUAA**

**Symptoms:**

* Token exchange succeeds but API calls fail with scope errors
* XSUAA token doesn't include expected scopes

**Solution:**

1. Review your `xs-security.json` configuration
2. Ensure required scopes are defined
3. Verify the IAS token includes the necessary claims for scope mapping
4. Check role collections are assigned to users in BTP Cockpit

***

### Stage 4: Cloud Connector & SAP Backend

**Q: Error "SSL handshake failed" or certificate errors in Cloud Connector logs**

**Symptoms:**

* Cloud Connector logs show SSL/TLS errors
* Connection to SAP backend fails before authentication

**Possible Causes & Solutions:**

| Cause                                  | How to Verify                                                     | Solution                                          |
| -------------------------------------- | ----------------------------------------------------------------- | ------------------------------------------------- |
| System certificate not generated       | Cloud Connector → Configuration → On Premise → System Certificate | Generate a self-signed system certificate         |
| CA certificate not created             | Cloud Connector → Configuration → On Premise → CA Certificate     | Generate CA certificate for principal propagation |
| SAP doesn't trust Cloud Connector cert | Check STRUST in SAP                                               | Import Cloud Connector CA certificate (Step 3.1)  |

***

**Q: SAP returns HTTP 401 or 403 even though certificates are configured**

**Symptoms:**

* Cloud Connector connects successfully
* SAP receives the request but rejects it with 401/403
* SMICM trace shows "intermediary is NOT trusted"

**Possible Causes & Solutions:**

| Cause                                    | How to Verify                                                | Solution                                                                           |
| ---------------------------------------- | ------------------------------------------------------------ | ---------------------------------------------------------------------------------- |
| ICM trust parameters not set             | RZ10 → Check profile parameters                              | Add `icm/HTTPS/trust_client_with_issuer` and `icm/HTTPS/trust_client_with_subject` |
| Parameter values don't match certificate | Compare RZ10 values with Cloud Connector certificate details | Copy exact Issuer and Subject DN from Cloud Connector system certificate           |
| ICM not restarted                        | Check SMICM parameter display                                | Restart ICM via SMICM → Administration → ICM → Exit Hard → Global                  |
| Profile not activated                    | RZ10 → Check profile status                                  | Save and activate profile, then restart ICM                                        |

**Diagnostic Steps:**

1. Run transaction **SMICM**
2. Go to **Goto → Trace File → Display End**
3. Look for entries containing "trust" or "certificate"
4. Check for messages like "intermediary is NOT trusted"

**Example SMICM trace showing the issue:**

```
HttpModIsReverseProxyTrustworthy: no trust relationship to intermediary specified
HttpModGetDefRules: intermediary is NOT trusted -> remove SSL header fields
```

***

**Q: SAP shows "User not found" or login fails after certificate validation**

**Symptoms:**

* Certificate is accepted (no SSL errors)
* But SAP cannot map certificate to a user
* SM21 system log shows login failures

**Possible Causes & Solutions:**

| Cause                               | How to Verify                        | Solution                                             |
| ----------------------------------- | ------------------------------------ | ---------------------------------------------------- |
| CERTRULE not configured             | Run CERTRULE transaction             | Create mapping rule (Step 3.3)                       |
| Rule-based mapping not enabled      | Check RZ10 parameters                | Set `login/certificate_mapping_rulebased = 1`        |
| Email mismatch                      | Compare Okta email with SAP USREMAIL | Ensure emails match exactly (case-sensitive)         |
| User missing email in SAP           | SU01 → User → Address tab            | Add email address matching Okta                      |
| Wrong certificate attribute in rule | CERTRULE → View rule details         | Ensure rule extracts CN and maps to Email login type |

**Diagnostic Steps:**

1. Run transaction **SM21** (System Log)
2. Filter by time of failed login
3. Look for certificate-related messages
4. Run **SE16** → Table **USREMAIL** to verify user email mappings

***

**Q: Authentication works but SAP shows wrong user or technical user**

**Symptoms:**

* API calls succeed
* But audit log shows a technical user instead of the actual user
* Authorization checks fail because wrong user context

**Possible Causes & Solutions:**

| Cause                                            | Solution                                                                                    |
| ------------------------------------------------ | ------------------------------------------------------------------------------------------- |
| Principal Propagation not enabled on destination | In BTP Cockpit, edit destination → Set Authentication to `PrincipalPropagation`             |
| Cloud Connector Access Control misconfigured     | Cloud Connector → Access Control → Edit mapping → Set Principal Type to `X.509 Certificate` |
| Subject pattern wrong                            | Cloud Connector → Principal Propagation → Verify Subject Pattern is `CN=${email}`           |

***

### General Diagnostics

**Q: Where can I find logs for each component?**

| Component           | Where to Find Logs                                             |
| ------------------- | -------------------------------------------------------------- |
| **Okta**            | Admin Console → Reports → System Log                           |
| **IAS**             | Admin Console → Monitoring & Reporting → Troubleshooting Logs  |
| **XSUAA/BTP**       | BTP Cockpit → Subaccount → Logs (if enabled)                   |
| **Cloud Connector** | Admin UI → Logs; or `ljs_trace.log` / `scc_core.trc` files     |
| **SAP ABAP**        | SMICM (ICM trace), SM21 (System log), STAD (Workload analysis) |
| **Looply**          | Contact Looply support for workflow execution logs             |

***

**Q: How do I enable detailed tracing in SAP ICM?**

**Steps:**

1. Run transaction **SMICM**
2. Go to **Goto → Trace Level → Set Trace Level**
3. Set level to **2** or **3** (3 = most detailed)
4. Reproduce the issue
5. Go to **Goto → Trace File → Display End**
6. Search for relevant entries (certificate, trust, SSL)

> ⚠️ **Important:** Reset trace level to 1 after debugging to avoid performance impact.

***

**Q: How can I test the token exchange chain step-by-step?**

Contact your Looply implementation team. We can provide:

* A Postman collection for testing each token exchange hop individually
* Step-by-step guidance on validating tokens at each stage
* Help decoding and inspecting JWT tokens at each hop

***

**Q: How do I decode and inspect a JWT token?**

**Option 1: Online (for non-sensitive tokens only)**

* Go to [jwt.io](https://jwt.io/)
* Paste the token to see header, payload, and signature

**Option 2: Command line**

```bash
# Decode the payload (middle section of the JWT)
echo "<middle-part-of-jwt>" | base64 -d | jq .
```

**Key claims to check:**

| Claim   | What to Verify                            |
| ------- | ----------------------------------------- |
| `iss`   | Issuer matches expected identity provider |
| `aud`   | Audience includes the target service      |
| `sub`   | Subject contains the user identifier      |
| `email` | Email matches SAP user's email            |
| `exp`   | Token hasn't expired                      |

***

### Still Need Help?

If you've worked through the relevant FAQ items and are still experiencing issues:

1. **Gather diagnostic information:**
   * Screenshot or copy of the error message
   * Relevant logs from the component where the failure occurs
   * Time of the failed request (for log correlation)
2. **Contact Looply support:**
   * Email: support@looply.ai
   * Include the diagnostic information above
   * Reference this FAQ and which items you've already checked
3. **For SAP-specific issues:**
   * Engage your SAP Basis team with the SMICM/SM21 traces
   * Reference SAP Note 2052899 for trusted proxy configuration
