# Looply 120 Service Pack 1 Change Log

## **🛠️ Improvements**



* From this version onwards, class methods replace functions as the standard approach to gather data (for Outbound or Resume) or to process an inbound call (for Inbound scenarios). If a "Class Name" is maintained in the Looply activity table for a certain scenario, the "Function Name" field will be ignored. You can create your own Z-class by implementing interface `/LOOPLY/IF_PROCESS_HANDLER`. The following methods are available:
  * `TRIGGER`: To gather data for outbound scenarios
  * `RESUME`: To gather data for resume scenarios
  * `POST`: For Inbound scenarios when a POST request is made from your Looply workflow to the `DataInSet` endpoint
  * `GET_DATA`: For Inbound scenarios when a POST request is made from your Looply workflow to the DataOutSet endpoint
  * `GET_FILE`: To return raw data, i.e. an attachment when a READ request is made to the AttSet endpoint



* A new odata endpoint `/looply/service_srv/DataOutSet` has been made available. A READ request can be made to this endpoint from Looply to gather data.



* A number of improvements were made around the Packaged Services module. The following objects are now available:<br>
  * Class `/LOOPLY/UTILS`. This contains methods to:
    * Forward a work item
    * Retrieve GOS attachment data
    * Update the Fiori Inbox of all approvers after a workitem update
    * Retrieve data regarding a certain workitem<br>
  *   Class `/LOOPLY/UTIL_PROCESS_HANDLER`. This implements interface `/LOOPLY/IF_PROCESS_HANDLER` and calls class `/LOOPLY/UTILS`. It can be used out of the box without any coding, simply by configuring entries in the Activity table and making a call with a matching Scenario, Scenario Id, Scenario Version and Step from the Looply workflow. The following scenario ids must be used:

      * Forward a work item: `FORWARD_WI`
      * Retrieve GOS attachment data: `GET_GOS_ATT`
      * Update the Fiori Inbox of all approvers after a workitem update: `UPDATE_INBOX`
      * Retrieve data regarding a certain workitem: `WI_GET_WF_DETAILS`&#x20;

      <figure><img src="../../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>


  * Enhancement implementation `/LOOPLY/WF_RESUME_ONFORWARD`. This implements BADI `WF_WI_FORWARD` and resumes the Looply workflow when a workitem is forwarded via the Fiori inbox. To activate this implementation, modify its filter values in transaction se19.<br>
  * The workflow approval history is now available in the standard `util_data` json string sent to Looply. This contains information on the actions taken and comments added by all agents.



## **🪲 Bug Fixes**



* [Looply Note 1](../correction-notes/note-1.md) is included in this release. This corrects an error where users got a Looply error when actioning, for example approving or rejecting, a standard Fiori app.
