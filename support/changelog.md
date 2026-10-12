# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

### **2.8.2 (28-08-2026)**

**🚀 New Features**

**Developer API**

* **OAuth 2.0 API Clients:** Create and manage API clients from Developer Tools → API Clients. Each client issues a client ID and secret usable against the OAuth token endpoint, so integrations can authenticate without a user session.
* **Token Endpoint:** New token endpoint at `api.looply.io/v1/oauth2` issuing short-lived bearer tokens for the Developer API.
* **Scoped Access:** Each client is granted specific scopes — `workflows:read`, `workflows:manage`, `workflows:execute`, `executions:read`, `executions:manage` and `apps:read`. Scopes are enforced on every Developer API route, so a client can only reach what it was granted.
* **Edit Scopes:** A client's scopes can be changed after creation without reissuing its credentials. The API Clients table shows all scopes held by each client.
* **Configurable Token Lifetime:** Set how long a client's tokens remain valid — anywhere from 15 minutes to 24 hours, defaulting to 1 hour. Adjustable per client at creation or afterwards.
* **Deactivate and Reactivate:** Clients can be deactivated and later reactivated. Deactivating revokes issued tokens immediately, rather than only preventing new ones from being issued.
* **Audit Log:** Each client has an audit log of its activity, browsable in a drawer with paged loading.
* **Execution Actions Endpoint:** New Developer API endpoint returning the action log for a workflow execution.
* **Role-Based Client Management:** API client management is available to Super Admins and Developers. Other roles cannot view or manage clients.

**Adaptive Card Step**

* **Failure Handling Modes:** The _Continue if action fails_ toggle has been replaced with three explicit modes, so you can choose how a step behaves when some recipients cannot receive the card:
  * **Strict** — the step fails if any recipient cannot receive the card. Recommended for approval steps and critical notifications.
  * **Tolerate partial failures** — the step succeeds if at least one recipient receives the card, with failures recorded as warnings.
  * **Best effort** — the step always succeeds, even if no recipients receive the card. Intended for fire-and-forget notifications only; avoid it where a wait, response or approval step follows, as an execution with zero deliveries may sit in RUNNING until terminated.
* **Existing Workflows Unchanged:** Workflows using the old toggle keep their current behaviour automatically — off becomes Strict, on becomes Tolerate partial failures. No action is needed.
* **Independent per Step:** Update and Delete steps set their own failure mode, so a strict Update can follow a Send that tolerated partial delivery. Recipients carried over from that Send are surfaced as warnings rather than failures.
* **Stuck Execution Banner:** Where a step completes with no deliveries and the execution may not progress, Workflow History now shows a banner with a one-click option to terminate the execution.

**Roles & Permissions**

* **Auditor Role:** A new read-only role with access to Workflow History. Auditors cannot change workflows, executions or organisation data, and cannot terminate executions.
* **Workflow Administrator Role:** A new role with read access to Workflow History, the ability to terminate in-progress executions, and access to Lifecycle Management for bulk operations on Teams messages.
* **Role Precedence by Privilege:** Where a user holds more than one role, the more privileged role now applies consistently.

**Team Members**

* **Invite-Only Sign-Up:** People now have to be invited to join your organisation rather than signing up themselves. An administrator sends the invitation from Team Members, and the recipient completes sign-up from the link in it.
* **Workspace Owner Marked:** The workspace owner is now identified with a star in the members table. Transferring ownership is handled by Looply support.
* **Protected Accounts:** Edit and disable actions are no longer offered on your own account or on the workspace owner's, preventing an administrator from locking themselves or the owner out.

**Lifecycle Management**

* **Archived Workflow Status:** Archived workflows are now shown and searchable alongside active ones.
* **Sort by Deletable:** The _Deletable?_ column is now sortable, with sorting applied across the full result set rather than the current page.
* **Improved User Search:** The user picker now finds people who have no mail address recorded in Microsoft Entra, matches addresses and user principal names regardless of case, and returns one result per person instead of several.
* **Former Names and Addresses:** A user's previous names and email addresses are now always shown, paired with each other, and search results explain which former value was matched.

**Workflow Designer**

* **Searchable Selectors:** The Select Card, Select App and Select Workflow Schema dropdowns now have a search box, with a clear button and a count of matching cards.

**Integrations**

* **Encrypted Custom Fields:** Custom connection parameters can be marked as passwords — secured on create and update, and masked in API responses.
* **Billing Plan Filtering:** Connected integrations are filtered by the organisation's billing plan, showing only what the plan includes.
* **Integration Tags:** Integrations now support a tag field on create and update.

**🛠️ Improvements**

**Sign-In**

* **Verification Challenge:** Sign-in now includes a brief verification step to confirm a genuine browser session. No action is required beyond completing the challenge on the sign-in page.

**Lifecycle Management**

* **Faster First Load:** Lifecycle Management now waits and retries rather than showing an error if the first request of the day takes longer than usual, with a brief loading hint while it does.

**Team Members**

* **Clearer Role Names:** The Admin role is now labelled _Super Admin_ to distinguish it from other administrative roles, and roles appear in a consistent order.

**User Settings**

* **Email Address:** The email address on a profile is no longer editable, as it identifies the account for sign-in.

**Developer API**

* **Clearer Validation Errors:** Invalid requests now name the specific parameter at fault instead of returning a generic message.
* **Streamlined Responses:** Workflow and execution responses now return only the documented fields. If an integration relies on a field not listed in the API reference, check it against the reference before upgrading.

**🪲 Bug Fixes**

**Lifecycle Management**

* **Notification Deletion:** Fixed deletion failing for notifications whose workflow execution had already been removed by a log retention policy.
* **Sort Order:** Fixed sorting not being applied to the deletion candidates table.

**Adaptive Card Step**

* **Update and Delete Recipients:** Fixed Update and Delete incorrectly succeeding for recipients who were never sent a card, and corrected the ordering so a replacement notification is sent before the original card is removed.

**App Manager**

* **Card Action Status:** Fixed a notification's status not updating correctly after someone acted on its card.

**Dashboard**

* **Workflows Widget:** Fixed an invalid modification date crashing the workflows widget on the home screen.

**Teams Messaging**

* **Message Delivery:** Fixed message delivery, card updates and bulk message deletion failing for some Teams configurations.

#### **2.8.1 (01-04-2026)**

#### **🚀 New Features**

**App Manager**

* **Installation Health Check (Beta):** New diagnostic tab that performs a multi-step health check against Microsoft Graph and Bot Framework — verifying app installation, conversation context, connection liveness, and bot accessibility for each user.
* **Blocked Bot Detection:** Health checks detect if a user has blocked the bot in Teams — surfaced as a clear "Blocked" status in the health table.
* **Self-Healing Conversations:** When a health check detects a stale conversation, Looply automatically re-registers it via the Bot Framework REST API — recovering the connection without manual reinstallation.
* **Test Message Delivery:** Admins can send a test message to individual users directly from the health table to verify end-to-end message delivery, with the option to delete the message afterwards.
* **Profile Sync:** New sync button in the Installation Health tab fetches the latest user profile (name, email, job title) from Microsoft Graph and updates the local record.
* **Activity Tracking:** Each admin action (health check, test message, profile sync, message deletion) is recorded with a timestamp and displayed in the health table for audit visibility.

**Adaptive Card Step**

* **Self-Healing Conversation Recovery:** If Microsoft Teams loses track of a conversation, the bot now automatically recovers and retries the operation. Previously, this resulted in a permanent "Conversation not found" failure requiring the end user to manually re-install the app. Applies to SEND, UPDATE, and DELETE actions.

#### **🛠️ Improvements**

**Adaptive Card Step**

* **Partial Delivery Tracking:** Workflow history now records the full picture when a step partially succeeds — including successful deliveries, the failure, and any skipped recipients.
* **Recipient Email in Error Messages:** Error messages now include the recipient email address for easier troubleshooting.
* **Accurate Step Error Messages:** Step errors now show the actual failure reason per recipient instead of a generic "Configuration error".
* **Successful DELETE Tracking:** Successful deletions are now recorded in workflow history, matching existing SEND and UPDATE behaviour.
* **Graceful Handling of Missing Cards:** UPDATE and DELETE steps now skip recipients whose original SEND failed, with a clear message instead of a cryptic error.
* **Missing Recipient Tracking:** UPDATE and DELETE steps now record recipients not found in the original SEND instead of silently dropping them.
* **Zero-Success Guard:** UPDATE and DELETE steps now fail when all recipients fail or are missing — matching existing SEND behaviour.

**Workflow History**

* **Workflow History Loading:** Extended retry strategy for Aurora cold starts with more attempts and longer delays, keeping the loading spinner active instead of showing an error.

#### **🪲 Bug Fixes**

**Workflow History**

* **Workflow Step Status:** Fixed a bug where failed workflow steps with warning logs were incorrectly displayed in amber instead of red on the history canvas.

**Adaptive Card Step**

* **"Continue if action fails" Toggle:** Fixed the toggle being bypassed for certain error types.
* **Delivery Record Preservation:** Fixed workflow history losing delivery records when a step fails — successful deliveries before the failure are now preserved.
* **DELETE Error Messages:** Fixed DELETE error messages showing a generic "Configuration error" instead of the actual error reason.

#### 2.8.0 (13-03-2026)

#### **🚀 New Features**

* **Dynamic Field Encryption:** Integration profiles now support marking custom connection parameter fields as passwords — these are automatically secured on create/update and masked in API responses
* **Billing Plan Filtering:** Connected organization integrations are now filtered by the organization's billing plan, only showing integrations included in the plan
* **Workflow Log Retention:** Configure automatic log retention policies per workflow with status-specific rules — keep successful runs for 30 days, keep failed runs for 90 days, or any combination you need
* **Organisation Default Retention:** Set a default log retention policy at the organisation level that applies to all workflows unless overridden
* **Execution Log Archiving:** Expired execution logs are automatically archived before purging, so you can still access historical data when needed
* **Workflow Soft-Delete:** Deleting a workflow now safely removes all associated executions and logs in the background, preventing orphaned data
* **Dashboard Time Period Filtering:** Filter dashboard stats by week, month, quarter, year, or all time with calendar-based boundaries (week starts Monday)
* **Dashboard User Preferences:** Save and persist per-user dashboard preferences for time period and archived execution visibility
* **Archived Executions in Dashboard:** Optionally include archived workflow executions in dashboard statistics and counts
* **TERMINATED Execution Status:** Track and display TERMINATED as a distinct execution status in dashboard stats
* **Bulk App Installation:** Install Teams apps across multiple users in bulk with background job processing
* **Bulk Install Job Management:** Query bulk install jobs by app with pagination, track per-user installed/requested status, and monitor job progress

#### **🛠️ Improvements**

**Workflow Designer**

* **Adaptive card step:** Custom integrations are now supported when sending and updating adaptive cards
* **Choice state:** Improved variable binding path resolution for more reliable conditional branching
* **HTTP Request step:** Added CSRF token support for SAP HTTP requests with secure token handling
* **Token exchange:** Support for chained token exchanges when integrating with services that require multi-step authentication, with cascade refresh strategy and individual hop expiry tracking
* **Single-hop token refresh:** Added Basic Auth support for single-hop OAuth token refresh flows
* **Custom integrations:** Added option to skip token refresh for integrations that don't support it

**Auth Service**

* **Teams SDK upgrade:** Migrated all auth pages to the latest Teams SDK
* **Auth UI:** Branded loading spinner and success checkmark on login completion instead of blank page flash
* **Token storage:** Simplified token storage for improved reliability and reduced payload size
* **Token expiry:** Added access token expiry tracking to the auth response

**Workflow History**

* **Pagination:** Archived execution counts are now included in pagination for a complete view of workflow history
* **Redriven executions:** Dashboard no longer overwrites execution status after a workflow is redriven, preserving the correct state

**Dashboard**

* **App Installation Counts:** Added pagination when querying app installations to ensure accurate counts for large datasets
* **Archived Workflow Executions:** Query support for retrieving archived execution data by organisation

#### **🪲 Bug Fixes**

* **Organization isolation:** Improved organization scoping on custom integration queries
* **Image URL field:** Fixed integration image handling when creating new versions
* **Integration profiles:** Authorization error messages now include context about which integration profile failed
* **Log retention:** Fixed format handling issues that could prevent the purge system from processing retention rules correctly
* **Redriven executions:** Stale cached archive data is now bypassed for redriven executions that are still in flight
* **Adaptive card removal:** Fixed card removal to prevent orphaned state on partial failures
* **Card update recipients:** Filter out recipients without a message ID to prevent card update failures
* **User mail normalization:** Improved email address normalization during bot member events
* **Member added handler:** Fixed user data handling when new members are added

### 2.7.0 (19-12-2025)

#### 🚀 New Features

* **Custom Integration Profiles:** Organizations can now create profiles for published custom integrations to connect with external systems.&#x20;
* Billing plan-based integration filtering to control access based on subscription tier

#### 🛠️ Improvements

* **Workflow Designer**
  * **Adaptive card step:** Now supports dynamic authentication to external third party system. A variety of authentication scenarios are covered.&#x20;
  * **HTTP Step:**&#x20;
    * Now supports custom integration binding for custom integrations
    * Basic authentication headers can be dynamically bound to environment, global, payload variables
* **Error Handling**
  * Improved error handling for failures
  * Improved error handling for bot framework
  * Better execution handling for long running processes&#x20;
* **Security**
  * Enhanced encryption for integration profiles
  * Improved monitoring infrastructure to track long running processes
  * Enhanced workspace authorisation checks

#### 🪲 Bug Fixes

* Fixed Teams message update timeout due to bot framework latency
* Fixed TTL calculation for debounce checks
* Fixed environment variable resolution for advanced conditional steps
* Fixed logic operator grouping for advanced conditionals
* Fixed rule group separation&#x20;
* Fixed type conversion for numeric values



### 2.6.0 (18-07-2025)

#### 🚀 New Features

* **Import/Export Workflows:** Easily transfer and manage your Looply workflows across different environments with our new import/export functionality
* **Mock Workflow Payloads:** Create mock payload data and use it in your Looply workflow executions to test/validate your workflows without live data
* **Mock Step Response:** Simulate outputs from  workflow steps for comprehensive testing/building without external dependencies or systems
* **Workflow Definition/Schema Editing:** Access your workflow definition/schema JSON code for advanced customisation/debugging&#x20;

### 2.5.0 (20-06-2025)

#### 🚀 New Features

**Notification Lifecycle Management**

* Organization admins can search & filter Looply notifications to manage their lifecycle with our manual clean-up process for selected or bulk deletions
* Simulate system notification deletions with dry run operations
* Monitor the status and progress of deletion requests in real-time with a comprehensive breakdown of each process and detailed statistics

#### 🛠️ Improvements

* Added support for conditionally sending workflow error notifications via payload, datastore or environment variable values
* Improved the resumeWorkflow developer API response with internal Looply codes
* Additional data added to Looply workflow error notification emails & webhook responses

### 2.4.0 (25-04-2025)

#### 🚀 New Features

**Workflow History Redesign**

A completely rebuilt Workflow History page with:

* Advanced filters by status and date range
* Improved pagination controls
* Enhanced search by process ID, workflow ID, and name
* Support for URL parameters to restore previous searches
* Easier navigation between newest and oldest records<br>

**Search Behaviour Update**

* Partial match search now requires an asterisk (\*) before or after the term. This replaces the previous automatic wildcard behaviour for better performance and control.

#### 🛠️ Improvements

* **Developer API:** Optimized performance & enhanced security measures when triggering, resuming & terminating workflows

#### 🪲 Bug Fixes

* Fixed an issue where attempting to send invalid adaptive card templates resulted in an incorrect 'App Not Available' error
* Fixed an issue with renaming datastores
* Fixed an issue where undefined/null values would cause an error when being evaluated in Advanced Conditional steps instead of acting as false values
* Fixed an issue where workflows would incorrectly detect an SAP integration as active even after all SAP integration steps were deleted, causing execution to be blocked if no SAP integration profile had been configured
* Fixed an issue where renaming workflows would not update until the page was refreshed
* Fixed an issue where binding HTTP/SAP request step URL attribute would appear as blank in text field
* Fixed alignment issues with multiple binding fields in a row in workflow step config editor layouts
* Fixed an issue where clicking on the + button in the workflow builder would not re-open the Toolbox
* Fixed an issue where Override Payload steps were injecting incorrect data in nested objects
* Fixed issues with triggerWorkflow API request when attempting to trigger/test newly created workflows or workflows with no executable steps
* Fixed an issue where users could not activate workflows when using Basic or Bearer Token authentication in HTTP Request steps

### 2.3.0 (20-03-2025)

#### 🚀 New Features

* **Global Datastores:** Create & manage shared data objects with Global Datastores which can be accessed across all your organization Looply workflows.
* **Workflow Environment Variables:** Define environment variable profiles within your workflows to inject environment-specific data in your processes at runtime.

#### 🛠️ Improvements

* Enhanced organization process ID usage when triggering or resuming workflow executions via developer API
* Improved the workflow step binding selection menu + added better support for nested object attribute binding

#### 🪲 Bug Fixes

* Fixed a bug where users could not version up scheduled workflows in paused status
* Fixed a bug where multiple rule conditions were not working as intended in Advanced Conditional steps
* Fixed an issue where workflows would crash & fail when workflow step bindings were undefined
* Fixed an issue where undefined workflow bindings were falling back to the invalid binding string instead of undefined value
* Fixed issues with the workflow step binding selection menu crashing when handling undefined values

### 2.2.2 (17-03-2025)

#### 🛠️ Improvements

* **Workflow Execution Storage:** Improved our process of storing workflow execution history logs for greater user control.

#### 🪲 Bug Fixes

* General bug fixes & improvements

### 2.2.1 (17-01-2025)

#### 🚀 New Features

* **Account Usage:** View your Looply organization usage data directly from the Looply dashboard with our new Account Usage page.
* **Billing Plans:** Stay up to date with your Looply subscription and monitor your quota allowance in real-time with Looply billing plans.

#### 🛠️ Improvements

* **Workflow History - Warning Logs:** Improved workflow history to highlight any warning logs thrown by your workflows regardless of success/failure.
* **Memory limit:** Increased Lambda memory limit to enable parallel processing to cater to concurrent workflow executions.
* **Debounce handling:** Enhanced debounce handling mechanism to manage rapid, multiple user actions
* **Error handling:** Improved error handling across Looply workflow to cascade runtime errors to developer.
* **Email notifications:** Organisational administrator will be notified when account quota is hit.
* **Error notifications:** Improved error reporting for workflow step failures.

#### 🪲 Bug Fixes

* Fixed a bug where connecting/disconnecting an integration did not correctly close the sidebar.
* Fixed a bug to handle user actions from android operating system.
* Fixed an  issue where updateAdaptiveCard was assuming actioned status if new card contained no action buttons.
* Fixed an issue with multiple recipients in adaptive card step, where the workflow step was failing despite having the 'Continue if action fails' flag set.

### 2.1.0 (29-07-2024)

#### 🚀 New Features

* **Advanced Conditional Steps:** New Advanced Conditional steps are now available in the workflow builder to support building complex if-else logic statements with multiple branches.
* **Workflow Error Notifications:** Workflows can now be configured to send notifications via email or webhook in the event of an error occurring during execution. Emails can be setup to be sent to one or more recipients every time an error occurs, or report any errors in batches every hour/day/week/month. Webhook requests support custom headers and basic authentication for secure communication.&#x20;

#### 🛠️ Improvements

* **Object Data Binding:** Data binding to entire objects from payload/schema is now supported in the workflow builder.
* **Adaptive Card - Subject/From Addresses:** Users can now set a subject & from address for adaptive card notifications which will be displayed with messages within the Looply Notifications Centre.
* **Workflow Configuration Error Status:** Workflows may now receive a `Configuration Error` status if an invalid workflow design is saved during development. E.g. workflow has no obvious termination point or contains conditions with no output branches to follow. Workflows with this status cannot be executed or activated.&#x20;

#### 🪲 Bug Fixes

* Fixed an issue where viewing workflow executions after testing steps resulting in a 404 error.
* Executions of draft workflows now correctly reflect the workflow steps at the time of the execution when viewing in workflow history.
* Fixed an issue where users could not view the execution history of workflows triggered from within another workflow.
* Fixed `SchemaValidation` error when updating an app with the notifications centre enabled.
* Fixed an issue where all HTTP/SAP request hardcoded body values were being treated as string.
* Fixed a bug where the adaptive card designer preview didn't update as changes were made until refreshed.
* Fixed an issue where executing incorrectly configured workflows would result in a previous version being executed instead.&#x20;
* Fixed an issue where incorrect binding on adaptive card designer could result in the black screen of death.

### 2.0.3 (23-04-2024)

#### 🪲 Bug Fixes

* Fixed an issue where the user was unable to create a new SAP integration profile using basic authentication.

### 2.0.2 (18-04-2024)

#### 🛠️ Improvements

* New warning dialog message to provide feedback when a workflow is saved with configuration errors.
* UI improvements on various screens for better user experience on smaller screens.
* Replaced Integrations sidebar with a new overlay sidebar to prevent UI issues on smaller screens.
* Changed where workflow history is stored/read from to support increased amount of workflow execution logs.
* Updated password error message to include minimum number of required characters.

#### 🪲 Bug Fixes

* Fixed an issue where SAP HTTP Requests were silently failing validation checks.

### 2.0.1 (26-03-2024)

#### 🛠️ Improvements

* **SAP OAuth2 URL:** Updated SAP integration configuration settings to now support OAuth2 authorisation outside of the host domain. As a result of this, the OAuth2 `host URL` field has been renamed `token URL` - allowing users to provide 2 different URLs for token and authorisation.&#x20;

### 2.0.0 (29-02-2024)

#### &#x20;🚀 New Features

* **New Workflow Steps:** Await Response, Redirect, Override Payload and Terminate steps are now available for use in the workflow builder.
* Looply Workflows now support multiple loops or skipping of steps.
* **Adaptive Card - Update/Delete Notify:** Users can now select an option to notify the recipients when their adaptive card is updated or deleted within Teams.&#x20;
* **Adaptive Card - More Detail Cards:**  Users can now enable & configure an expanded detailed card to be attached to their initial adaptive card within Teams which opens in a pop-up window via a button click.&#x20;
* View specific run data within Workflow History when steps have executed multiple times.
* Workflows are now validated at the point of activation to prevent missing step data.
* **Adaptive Card Designer:** Added functionality to swap between card versions & toggle between different workflow schemas for data binding.
* **Workflow Redirect Lines**: Redirect steps now implement new smart edge functionality which prevents connections from colliding with other steps.&#x20;
* **Terminate Executions:** Users can now terminate in progress workflow executions within the Workflow Execution screen.

#### 🛠️ Improvements

* **Multiple Adaptive Card Recipients:**  Users can now enter multiple recipients when sending adaptive cards within workflows which can be manually entered or bound from previous step data.
* Adaptive Card `SEND` action steps now allow for Teams or External responses to be added independently of each other (previously required both).&#x20;
* **Workflow Adaptive Card Version Selection:** Users can now choose to select `DRAFT` card versions or select an option to always use the latest version when sending adaptive cards via workflow.&#x20;
* Workflow Adaptive Card action step responses have been changed from running horizontally to linear within workflows.
* Users can now trigger the current workflow using Trigger Workflow steps (previously not supported).
* Workflow History: list view has been changed to allow more logs and includes pagination.
* Adaptive Card Designer: users now have the option to delete specific card versions.
* General workflow builder UI improvements.

#### 🪲 Bug Fixes

* Fixed an issue where users could add workflow steps with multiple branches in the middle of 2 steps (creating conflicts).
* Fixed an issue where workflow step names were not being displayed or were incorrectly cased.
* Fixed an issue where workflow last modified date was incorrect.&#x20;
* Resolved an issue where the Adaptive Card Designer & WFB Function Code Editor themes were conflicting with each other.&#x20;

### 1.1.0 (07-11-2023)

#### 🚀 New Features

* New search feature for the User Manager Tab within the Looply App creation screen.

#### 🛠️ Improvements

* Changed the location of the final Deploy button within the Looply App creation screen.
* Improved the way Looply fetches users from your teams account.
* Increased number of users shown per page on the User Manager and Install History tabs within the Looply App creation screen.
* Disabled Help section navigation options (Not yet available).
* Restyled the description tab within the Microsoft Teams integration sidebar.

#### 🪲 Bug Fixes

* Fixed an issue where installing a Looply App to any user would not update the Install History tab.

### 1.0.0 (16-10-2023)

Initial Looply alpha version featuring authenticated Looply dashboard with adaptive card designer, workflow builder, workflow history, integrations and app creation.

#### 🚀 Features

* Dashboard 2FA authentication + ability to login with TOTP authenticators
* Integrated Microsoft Adaptive Card Designer
* Looply Workflow Builder
* View Workflow History using Looply's Execution History screen
* Looply Integrations - supporting Microsoft + SAP
* Create, customise and deploy Microsoft Teams apps
* Looply Developer API + manage/create API keys
* Create and manage organisations within Looply
* Account home dashboard with usage analytics
