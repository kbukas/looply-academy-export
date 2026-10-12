---
description: >-
  In order to integrate Looply with SAP, you must first install the delivered
  ABAP Looply Add-On in your ECC or S/4HANA system.
---

# Installing the ABAP Looply Add-On

## System Requirements

The following software components are required:

* SAP\_ABA (minimum supported release is 750)
* SAP\_BASIS (minimum supported release is 750)

## Performing the installation&#x20;

The Add-On can be installed using the standard SAP Add-On Installation Tool (SAINT). For detailed information on the SAINT tool, please see the [Installing and Upgrading Add-Ons ](https://help.sap.com/saphelp_nwmobile71/helpdata/en/18/e08d38dfc44765e10000009b38f842/content.htm)section of the SAP help portal. A quick overview of the required steps is also provided bellow:

* Logon to client 000 of your SAP system as a user that has SAP\_ALL authorization. Do not use the SAP\* or DDIC user.
* Go to transaction SAINT.
* Select Installation Package->Load Packages->From Front End and browse to the delivered .SAR file.
* Go to transaction SGEN.
* Select “Generate All Objects of Selected Software Components” and click Continue.
* Select the “Looply” software component and click Continue.
* Select for Parallel Generation: Leave default. Click Continue.
* Start Job Directly.
* Generation speed depends on your system but should take under 10 minutes.

Please note that all the above steps must first be performed in your development system and then repeated throughout your landscape.

## Post-Installation Tasks

After installing the Add-On, the following tasks must be performed:

### Create RFC Connection&#x20;

In transaction sm59, create an RFC connection to Looply. This will be used by the SAP system to trigger (or Resume) Looply workflows. Create a connection with the following details: (This step should be performed in every system in your landscape)

**Connection Type:** G\
**Target Host:** api.looply.io\
**Path Prefix:** /v2/workflows      &#x20;

<figure><img src="../../../.gitbook/assets/sm59 screenshot.png" alt=""><figcaption></figcaption></figure>

In the _Logon & Security_ tab, set SSL to active and select the correct SSL Certificate:&#x20;

<figure><img src="../../../.gitbook/assets/sm59 screenshot 2.png" alt=""><figcaption></figcaption></figure>

At this point you may need to whitelist the Looply host so that i is not blocked by your firewall. When attempting a _Connection Test_ you should get a 403 _Forbidden_ response. You can ignore the 403 error

### Gateway Service Set-up

In this step we will configure the SAP Gateway service used by Looply to post data to SAP. The step depends on your setup. If you are running Gateway on the same system Looply is installed in, follow the steps in the [Gateway Service Setup - Single System](gateway-service-setup-single-system.md) page. If you are running Gateway on a Fiori Hub system, follow the steps in the [Gateway Service Setup - Hub scenario](gateway-service-setup-hub-scenario.md) page.

### Create API key

In the Looply web app, create an API key by going to the _API keys_ tab and clicking on the _Create API Key_ button.

<figure><img src="../../../.gitbook/assets/Create API key.png" alt=""><figcaption></figcaption></figure>

### Configure System Settings

On your SAP system, go to the following transaction in SPRO and maintain an entry for each of the systems in your landscape:

General Application Functions -> Looply -> System Settings

* **SAP System ID:** The ID of your SAP system
* **RFC Destination:** The name of the sm59 connection created in the previous step&#x20;
* **API Key:** The API key created in the previous step (you can use the same API key for each system in your landscape)
* **Gateway System ID:** The ID of the corresponding Gateway System (if you are running Gateway on the same system Looply is installed in, you can leave this field blank)&#x20;
* **Gateway System Client:** The client of the corresponding Gateway System (if you are running Gateway on the same system Looply is installed in, you can leave this field blank)&#x20;
* **Profile:** Environment profile to be used when triggering a workflow (see [Environment Variables & Profiles](https://academy.looply.ai/workflows/environment-variables-and-profiles) )
* **Fiori Launchpad Host:** A common requirement is to include a url (or button) to a Fiori app on your Looply card. This field may be used to store the host used to construct the url.  &#x20;

These entries should then be transported throughout your landscape.

### Maintain Number Range Intervals

In transaction snro, maintain intervals for number range /LOOPLY/EI as follows: \
**Number Range No.**       01\
**From No.**                         0000000001\
**To Number**                       9999999999

This step must be repeated in every system of your landscape.

### &#x20;SAP Workflow Integration

If you wish to integrate Looply with SAP workflow, also follow the steps in [this section](../sap-workflow-integration.md#set-up).
