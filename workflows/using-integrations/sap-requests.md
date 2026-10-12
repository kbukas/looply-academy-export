---
description: Sending HTTP Requests to your integrated SAP system with Looply workflows
---

# SAP Requests

Seamlessly integrate SAP Requests within your workflows, establishing a direct communication channel with your configured SAP systems.&#x20;

Here, the power of HTTP Requests becomes your strategic tool, enabling seamless updates within your SAP environment triggered by specific workflow events. This documentation guides you through the precise steps, ensuring you unlock the full potential of SAP Requests for a synchronized and efficient workflow experience.

## Configuring Requests

You can get started configuring your adaptive card action by clicking on a **+** button in your workflow and selecting the **SAP Request** step from the Workflow's toolbox.  &#x20;

SAP Requests sent from Looply support the following configuration options:&#x20;

* SAP Profile or System ID & Client ID
* Service Path
* Entity Name
* Request Type
  * `GET`
  * `POST`
  * `PUT`
  * `PATCH`
  * `DELETE`
* URL Parameters
* Headers
* Body Type
  * `JSON`
  * `multipart/form-data`
  * `x-www-form-urlencoded`
  * `XML`
  * `Raw`
* Body

### SAP System

Take control of your HTTP Requests by specifying the destination SAP system. This crucial step ensures that your request reaches the intended target, enabling precise and effective communication with your configured SAP environment.

This can be done through pre-configured SAP Profiles or by manually entering system credentials.&#x20;

#### Profiles

Use the **SAP Profile** dropdown menu to view and select any of your pre-configured SAP Profiles that you have created and integrated with Looply.&#x20;

See [sap-integration](../../integrations/sap-integration/ "mention") for more information on creating SAP Profiles.&#x20;

#### Manual System Credentials

Alternatively, you can choose to manually enter a system ID and client ID for your SAP system by toggling the **Enter manual system credentials** switch.&#x20;

This can be useful if you wish to communicate with multiple different SAP systems and determine system credentials at runtime with data binding.&#x20;

For example, you might wish to specify your system ID in your workflow payload instead and bind it here.&#x20;

To enter or bind manual system credentials you can use the **System ID** and **Client ID** input fields which will become available once the switch has been toggled. Data from your workflow payload or previous steps can be accessed in these fields using the dropdown menu within.&#x20;

{% hint style="info" %}
**Note:** You must configure an SAP Profile for every system you attempt to communicate with, even if using manual system credentials. See [sap-integration](../../integrations/sap-integration/ "mention") for more information on creating SAP Profiles.&#x20;
{% endhint %}

#### Service Path

Use the **Service Path** input field to enter the path of the specific service within the system you are requesting.&#x20;

#### Entity Name

Use the **Entity Name** input field to enter the entity name you wish to target.&#x20;

### Request

You can specify the type of request to be sent by selecting the desired **request method** from the dropdown menu. Choose from `GET`, `POST`, `PUT`, `PATCH`, or `DELETE`. Then, in the **Request URL** text field, enter the accurate destination URL to which you want to send the request. Make sure the URL is correctly formatted to ensure successful communication.

#### URL Parameters

Users are also able to add query string parameters in their request if they require them under the **URL Parameters** section.&#x20;

Add specific query string parameters by entering their names and values in the corresponding **Key** and **Value** inputs. Need more parameters? Simply click **+** **Add parameter** for additional sets of inputs. This flexibility empowers you to tailor your request to meet specific criteria.

#### Headers

Additionally, users can specify specific headers they wish to attach to their request under the **Headers** section.&#x20;

Add headers by entering their names and values in the corresponding **Key** and **Value** inputs. Click the **+** **Add header** button for inserting more than one header.&#x20;

### Payload

The **Body** section allows you to define the payload of your HTTP request. Select the appropriate 'Body Type' from the dropdown menu - `JSON`, `multipart/form-data`, `x-www-urlencoded`, `XML`, or `Raw`.&#x20;

For `JSON`, `multipart/form-data`, and `x-www-urlencoded`, use the **key-value pair inputs** to finely tune your payload - using the **+ Add item** button to insert more than one data item.&#x20;

Alternatively, for `XML` or `Raw` data, leverage the **text area** to input your content directly.&#x20;

### Failure Scenarios

Navigate through potential hiccups effortlessly - Looply equips you with options to gracefully handle failures in your SAP Requests, offering the ability to continue on failure and automatic retry functionalities.

#### Continue on Failure

Looply supports the ability to continue executing your workflow in the event the SAP Request step fails. This can be useful if you wish to manually handle any errors or if your workflow is not reliant on the response from the request.&#x20;

This functionality can be enabled by toggling the **Continue if request fails** switch within the SAP Request editor.&#x20;

#### Automatic Retry

Additionally, users can enable automatic retries for their SAP Request where in the event of request failure - Looply will automatically attempt the request again. This feature is currently limited to a maximum of 100 attempts before the step will fail.&#x20;

To enable automatic retries, toggle the **Auto-retry** switch within the SAP Request editor.&#x20;

## Testing Requests

SAP Requests can be effortlessly validated by configuring your request data and using the Test Step feature.&#x20;

Simply click the **Test Step** button in the header of the SAP Request editor, execute your request, and receive instant feedback.&#x20;

The response status and any returned output will be displayed, ensuring a seamless testing experience within the confines of your workflow.
