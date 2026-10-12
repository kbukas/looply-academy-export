# Looply Implementation Plan

This guide outlines the standard implementation phases for deploying Looply within your organization. Whether you are integrating only Microsoft Teams or extending to SAP workflows, this phased approach helps ensure a smooth, efficient rollout.

Looply provides project templates, onboarding workshops, and technical assistance throughout your journey.

### Download detailed implementation plan &#x20;

<table data-view="cards"><thead><tr><th></th><th data-type="files"></th></tr></thead><tbody><tr><td>General Integration plan (without SAP Plugin)</td><td><a href=".gitbook/assets/Download Plan.xlsx">Download Plan.xlsx</a></td></tr><tr><td>SAP Integration plan (with SAP Plugin)</td><td><a href=".gitbook/assets/Download Plan-SAP.xlsx">Download Plan-SAP.xlsx</a></td></tr></tbody></table>

### Phase 1: Mobilization

| Task                                                            | Primary Owner |
| --------------------------------------------------------------- | ------------- |
| Appoint Customer Project Lead                                   | Customer      |
| Mobilize SAP Basis Team (if SAP integration planned)            | Customer      |
| Arrange access to Microsoft Teams Admin Center and Azure Portal | Customer      |
| Confirm Looply Solution Architect engagement (if applicable)    | Looply        |

***

### Phase 2: Access Setup

| Task                                                                           | Primary Owner     |
| ------------------------------------------------------------------------------ | ----------------- |
| Sign up for Looply organization account                                        | Customer          |
| Invite Admins and Developers                                                   | Customer          |
| Connect Azure AD tenant to Looply                                              | Customer          |
| Deploy Looply custom app into Teams Admin Center or organizational App Catalog | Customer IT       |
| Schedule optional onboarding workshop with Looply Solutions Engineer           | Looply (optional) |

***

### Phase 3: SAP Integration Setup _(If Applicable)_

| Task                                                                  | Primary Owner        |
| --------------------------------------------------------------------- | -------------------- |
| Install Looply ABAP Add-on on SAP Gateway                             | Customer (SAP Basis) |
| Configure SAP OAuth 2.0 Client                                        | Customer (SAP Basis) |
| Expose SAP OData and OAuth endpoints externally (direct or via proxy) | Customer (SAP Basis) |
| Map Azure AD user emails with SAP SU01 email fields                   | Customer (SAP Basis) |

***

### Phase 4: Workflow Development & Testing

| Task                                                               | Primary Owner         |
| ------------------------------------------------------------------ | --------------------- |
| Build initial test workflows (pilot)                               | Looply/Customer       |
| Design Adaptive Cards for workflows                                | Looply/Customer       |
| Conduct internal UAT (User Acceptance Testing) with pilot group    | Customer Project Team |
| Looply Solutions Engineer provides best practice review (optional) | Looply (optional)     |

***

### Phase 5: Go-Live Preparation

| Task                                                   | Primary Owner               |
| ------------------------------------------------------ | --------------------------- |
| Finalize production deployment plan                    | Customer Project Manager    |
| Complete internal IT security and compliance approvals | Customer IT / Security Team |
| Validate error handling, monitoring, and workflow logs | Customer IT / Admins        |

***

### Phase 6: Production Launch and Hypercare

| Task                                                                | Primary Owner                               |
| ------------------------------------------------------------------- | ------------------------------------------- |
| Move workflows to Production status                                 | Customer Admin                              |
| Announce Looply availability to users (internal comms)              | Customer Communications Team                |
| Monitor production workflows for first 2-4 weeks (hypercare period) | Customer Admins / Looply Support (optional) |
| Hold retrospective and plan for further enhancements                | Customer Project Team                       |

***

### Notes

* SAP-related steps apply only if SAP workflow integration is part of your project scope.
* Looply Support ([support@looply.ai](mailto:support@looply.ai)) is available for optional onboarding sessions, integration assistance, and go-live hypercare support.

***

### Implementation Timeline&#x20;

> Timelines may vary based on the complexity of workflows, number of SAP systems, and customer internal readiness.

***

### Project Roles and Responsibilities

| Role                      | Responsibilities                                                                  |
| ------------------------- | --------------------------------------------------------------------------------- |
| Customer Project Manager  | Internal coordination, stakeholder updates                                        |
| IT Admin / Azure Admin    | Azure Tenant Connection, App Deployment                                           |
| SAP Basis Team            | ABAP Add-on installation, OAuth setup, Gateway exposure                           |
| Looply Solutions Engineer | Technical guidance, SAP/Teams integration support, workflow design best practices |
| End-User Champions        | Pilot testing, feedback during UAT                                                |

***
