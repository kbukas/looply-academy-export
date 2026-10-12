---
hidden: true
---

# Go-Live Readiness Checklist

This checklist helps ensure that all required steps are complete before moving Looply into production.\
It complements the broader Looply Implementation Plan by focusing specifically on final environment validation.

Use this checklist to review Microsoft Teams setup, Azure integration, SAP configuration (if applicable), network access, and internal readiness.

***

| Phase                           | Checklist Item                                                                     | Owner                | Status (Ready/Not Ready) |
| ------------------------------- | ---------------------------------------------------------------------------------- | -------------------- | ------------------------ |
| Mobilization                    | Project team mobilized (Admins, Azure Admins, SAP Basis team)                      | Customer             | ⬜                        |
| Mobilization                    | Teams Admin Center access confirmed                                                | Customer             | ⬜                        |
| Microsoft 365 Setup             | Looply organization account created                                                | Customer             | ⬜                        |
| Microsoft 365 Setup             | Azure AD tenant connected to Looply                                                | Customer             | ⬜                        |
| Microsoft 365 Setup             | Admin consent granted for Graph API permissions                                    | Customer             | ⬜                        |
| Microsoft 365 Setup             | Looply custom app deployed into Teams Admin Center / App Catalog                   | Customer             | ⬜                        |
| Microsoft 365 Setup             | Test notification sent and received by a test user                                 | Customer             | ⬜                        |
| SAP Integration (If Applicable) | Looply ABAP Add-on installed and transported into SAP Gateway                      | Customer (SAP Basis) | ⬜                        |
| SAP Integration (If Applicable) | OAuth 2.0 client configured and tested in SAP Gateway                              | Customer (SAP Basis) | ⬜                        |
| SAP Integration (If Applicable) | OData and OAuth endpoints securely exposed externally (direct or proxy)            | Customer (SAP Basis) | ⬜                        |
| SAP Integration (If Applicable) | SAP user email mapping with Azure AD verified                                      | Customer (SAP Basis) | ⬜                        |
| SAP Integration (If Applicable) | End-to-end workflow tested from Teams ➔ SAP ➔ confirmation                         | Customer             | ⬜                        |
| Authentication Testing          | Basic Authentication flow tested (if applicable)                                   | Customer             | ⬜                        |
| Authentication Testing          | OAuth 2.0 Code Grant flow tested (if applicable)                                   | Customer             | ⬜                        |
| Authentication Testing          | SAML2.0 OBO flow tested (if applicable)                                            | Customer             | ⬜                        |
| Network & Security              | Outbound HTTPS access confirmed from SAP to Looply APIs                            | Customer IT          | ⬜                        |
| Network & Security              | TLS encryption validated (TLS 1.2+)                                                | Customer IT          | ⬜                        |
| Network & Security              | (Optional) Static IP allowlisting configured if needed                             | Customer IT          | ⬜                        |
| Testing & Validation            | Pilot group workflow testing completed successfully                                | Customer             | ⬜                        |
| Testing & Validation            | Error handling and audit logs verified inside Looply dashboard                     | Customer             | ⬜                        |
| Final Sign-Offs                 | IT operational readiness approved                                                  | Customer             | ⬜                        |
| Final Sign-Offs                 | Project stakeholders approved go-live                                              | Customer             | ⬜                        |
| Support Setup                   | Looply Support contacts documented ([support@looply.ai](mailto:support@looply.ai)) | Customer             | ⬜                        |
| Support Setup                   | Post-Go-Live monitoring plan established (optional)                                | Customer             | ⬜                        |

***

✅ Once all checklist items are complete, you are ready to officially move Looply into production and empower users with modern, intelligent workflow approvals inside Microsoft Teams.

> 📌 Need additional assistance for Go-Live day support? Contact [support@looply.ai](mailto:support@looply.ai) to coordinate.
