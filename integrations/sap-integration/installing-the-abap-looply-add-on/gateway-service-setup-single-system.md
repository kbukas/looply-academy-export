# Gateway Service Setup - Single System

## Manage SAP System Alias

Manage SAP System Alias and make sure the SAP System Alias is assigned to LOCAL.

* In NW 7.0-731 go the IMG in transaction SPRO, and open SAP NetWeaver->Gateway->OData Channel->Configuration->Connection Settings->SAP Gateway to SAP System->Manage SAP System Aliases
* In NW 740+ go to the IMG in transaction SPRO and open SAP Netweaver->SAP Gateway->OData Channel->Configuration->Connection Settings->SAP Gateway to SAP System->Manage SAP System Aliases
* Alternatively you could also directly edit table /IWFND/V\_DFSYAL in sm30.

<figure><img src="../../../.gitbook/assets/Manage SAP System Alias.png" alt=""><figcaption></figcaption></figure>

* **This step must be repeated in every system in your landscape.** \
  _&#x53;ee:_ [_http://scn.sap.com/docs/DOC-42241_](http://scn.sap.com/docs/DOC-42241)&#x20;

## Activate SAP Gateway

Activate SAP Gateway in every system in the landscape (if it is not already activated).

* In NW 7.0-731 go the IMG in transaction SPRO ->SAP Netweaver->Gateway->OData Channel->Configuration->Activate or Deactivate SAP Gateway
* In NW 740+ go to the IMG in transaction SPRO->SAP Netweaver->SAP Gateway->OData Channel->Configuration->Activate or Deactivate SAP Gateway if you are on a NW 740 system) and make sure SAP Gateway is active.

## Assign Alias to oData Service

For the alias defined in the above step, create an entry in table /IWFND/V\_MGDEAM (via transaction sm30) for service /LOOPLY/SERVICE\_SRV\_0001.

<figure><img src="../../../.gitbook/assets/Assign Alias to oData Service.png" alt=""><figcaption></figcaption></figure>

## oData Service Authorization

In order for users to be able to post data to SAP (to approve a card for example), they must have authorization for the underlying oData service. To add this to a (new or existing) role, do the following:

* Add authorization object s\_service to the role
* Click on the “change” button next to “Program, transaction or functi”:

<figure><img src="../../../.gitbook/assets/Screenshot 1.png" alt=""><figcaption></figcaption></figure>

* Select “TADIR Service” in the “Type” drop-down and add the following two entries: \
  R3TR    IWSG   /LOOPLY/SERVICE\_SRV\_0001\
  R3TR    IWSV    /LOOPLY/SERVICE\_SRV                0001

<figure><img src="../../../.gitbook/assets/Screenshot 2.png" alt=""><figcaption></figcaption></figure>



## Transport Changes

Transport all the changes made and new objects created in the above steps (including table entries) throughout your landscape.

## Activate sicf Node

In every system in your landscape, go to transaction SICF and activate the following node:

**/sap/opu/odata/looply/service\_srv**
