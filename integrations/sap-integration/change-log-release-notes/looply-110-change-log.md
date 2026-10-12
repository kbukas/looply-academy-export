# Looply 110 Change Log

## 🚀 New Features

* Packaged Services: SAP Workflow, Varo, or other system events can now be utilised to trigger or resume Looply with minimal coding effort. This is achieved by delivering a number of classes that can be used as an event receiver (via transaction SWETYPV) when an event is triggered.
* Users can now set an environment profile (see [Environment Variables & Profiles](../../../workflows/environment-variables-and-profiles.md) ) when triggering a workflow. This can be passed into import parameter IM\_PROFILE of method /LOOPLY/CORE=>TRIGGER\_WF or it can be configured in the "System Settings" IMG transaction.
* New import parameter IM\_RETRY has been added to method /LOOPLY/CORE=>RESUME\_WF. If this is set and the resume fails because the workflow is not at a wait state, the resume will be attempted again (up to a maximum of 10 times) after a 1 second interval.
* New method /LOOPLY/CORE=>IS\_APP\_INSTALLED was created. This calls the [getAppUserInstallationStatus API](../../../api-reference/workflow-api.md#getappuserinstallationstatus) to check if a Teams app is installed for a certain user.&#x20;



