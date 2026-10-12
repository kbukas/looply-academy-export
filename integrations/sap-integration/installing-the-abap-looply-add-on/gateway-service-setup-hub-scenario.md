---
description: >-
  The following steps must be performed on your gateway hub system. If you are
  you are running Gateway on the same system Looply is installed in, you may
  ignore this page.
---

# Gateway Service Setup - Hub scenario

## Create RFC Destination

If one does not already exist, create an SM59 destination which points from the gateway hub to the system where Looply is installed. It is recommended that a destination with the same name is created in all systems of the landscape (pointing to the relevant system each time) so that only 1 configuration entry covering all systems is created during the following steps.

## Create SAP System Alias

Create a System Alias using the RFC destination created in the previous step.

* In NW 740+ go to the IMG in transaction SPRO and open SAP Netweaver->SAP Gateway->OData Channel->Configuration->Connection Settings->SAP Gateway to SAP System->Manage SAP System Aliases
* In NW 7.0-731 go the IMG in transaction SPRO, and open SAP NetWeaver->Gateway->OData Channel->Configuration->Connection Settings->SAP Gateway to SAP System->Manage SAP System Aliases
* Alternatively you could also directly edit table /IWFND/V\_DFSYAL in sm30.

<figure><img src="../../../.gitbook/assets/create SAP System Alias.png" alt=""><figcaption></figcaption></figure>

## Maintain SAP Gateway Settings

Maintain SAP Gateway Settings using the RFC Destination and Alias created in the previous steps.

* In NW 7.0-731 go the IMG in transaction SPRO, and open SAP NetWeaver->Gateway Service Enablement->Backend OData Channel->Connection Settings to SAP Gateway->SAP Gateway Settings
* In NW 740+ go to the IMG in transaction SPRO and open SAP NetWeaver->SAP Gateway Service Enablement->Backend OData Channel->Connection Settings to SAP Gateway->SAP Gateway Settings
* Alternatively, you could also directly edit table /IWBEP/C\_SYSTEM in SM30.

<figure><img src="../../../.gitbook/assets/SAP GW settings 2.png" alt=""><figcaption></figcaption></figure>

## Activate SAP Gateway

Activate SAP Gateway in every system in the landscape (if it is not already activated).

* In NW 7.0-731 go the IMG in transaction SPRO ->SAP Netweaver->Gateway->OData Channel->Configuration->Activate or Deactivate SAP Gateway
* In NW 740+ go to the IMG in transaction SPRO->SAP Netweaver->SAP Gateway->OData Channel->Configuration->Activate or Deactivate SAP Gateway if you are on a NW 740 system) and make sure SAP Gateway is active.

## Create Package

In transaction se21, create package ZLOOPLY\_SERVICE.

## Add Service

In transaction /N/IWFND/MAINT\_SERVICE click on “Add service”. Then insert your alias in the "System Alias" field, "/LOOPLY/\*" in the "Technical Service Name" field and click on the “Get Services” button:

<figure><img src="../../../.gitbook/assets/Add Services.png" alt=""><figcaption></figcaption></figure>

Select the table row and click on "Add Selected Services". Here you may change the technical names for service and model. Enter the package created in the previous step and click on the "Enable OAuth for Service" checkbox:

<figure><img src="../../../.gitbook/assets/Add Services 2.png" alt=""><figcaption></figcaption></figure>

Back in transaction /N/IWFND/MAINT\_SERVICE, select the newly created service and click on "SAP Gateway Client" to test it:

<figure><img src="../../../.gitbook/assets/test service.png" alt=""><figcaption></figcaption></figure>

## oData Service Authorization <a href="#odata-service-authorization" id="odata-service-authorization"></a>

In order for users to be able to post data to SAP (to approve a card for example), they must have authorization for the underlying oData service. To add this to a (new or existing) role, do the following:

**In your Gateway hub system:**

* Add authorization object s\_service to the role
* Click on the “change” button next to “Program, transaction or functi”:

<figure><img src="../../../.gitbook/assets/Screenshot 1.png" alt=""><figcaption></figcaption></figure>

* Select “TADIR Service” in the “Type” drop-down and add the following entry: \
  R3TR IWSG ZSERVICE\_SRV\_0001

**In your ECC system:**

* Add authorization object s\_service to the role
* Click on the “change” button next to “Program, transaction or functi”:
* Select “TADIR Service” in the “Type” drop-down and add the following entry: \
  R3TR    IWSV    /LOOPLY/SERVICE\_SRV                0001

## Transport changes

Transport all the changes made and new objects created in the above steps (including table entries) throughout your landscape.

## Activate sicf node

**In the system where Looply is installed**,  go to transaction SICF and activate the following node:

**/sap/opu/odata/looply/service\_srv**

