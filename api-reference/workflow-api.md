---
description: All Looply Workflow related API endpoints
---

# Workflow API

## triggerWorkflow

<mark style="color:green;">`POST`</mark>`https://api.looply.io/v2/workflows/triggerworkflow/{workflow_id}/{workflow_version}`

The `triggerWorkflow` endpoint initiates the execution of a specified workflow. When called, this endpoint triggers the workflow process, allowing you to automate various tasks and operations defined within the workflow. This endpoint can receive a JSON body payload to be passed to the workflow execution.  A `looply_execution_id` will be returned on successful trigger of the workflow.

> AWS IP address for whitelisting Looply API calls: **52.208.220.68**

#### Path Parameters

| Name                                           | Type   | Description             |
| ---------------------------------------------- | ------ | ----------------------- |
| workflow\_id<mark style="color:red;">\*</mark> | string | id of the workflow      |
| workflow\_version                              | string | version of the workflow |

#### Query String Parameters

| Name    | Type   | Description                                                                                                                                      |
| ------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| profile | string | name of environment variable profile (see [environment-variables-and-profiles.md](../workflows/environment-variables-and-profiles.md "mention")) |

#### Headers

| Name                                        | Type   | Description    |
| ------------------------------------------- | ------ | -------------- |
| x-api-key<mark style="color:red;">\*</mark> | string | Looply API Key |

{% tabs %}
{% tab title="200: OK Success Response" %}
```json
{
	"message": "success",
	"looply_execution_id": "****",
	// Optional: only included in response when a profile is specified
	"profile": "prod"
}
```
{% endtab %}

{% tab title="403: Forbidden Incorrect or No API Key Supplied" %}

{% endtab %}

{% tab title="500: Internal Server Error" %}
Serverside issue with this endpoint

```json
{
    "message": "something went wrong"
}
```
{% endtab %}

{% tab title="400: Bad Request" %}
Workflow ID is missing from the request path

```json
{
    "message": "incorrect workflow id"
}
```
{% endtab %}
{% endtabs %}

## resumeWorkflow

<mark style="color:green;">`POST`</mark> `https://api.looply.io/v2/workflows/resumeWorkflow/{process_id}`

Manually resume workflows depending on the process\_id supplied. Resuming a workflow can contain any JSON stringified body payload, `source` is the only protected attribute.&#x20;

#### Path Parameters

| Name                                          | Type   | Description                                                                                            |
| --------------------------------------------- | ------ | ------------------------------------------------------------------------------------------------------ |
| process\_id<mark style="color:red;">\*</mark> | string | `process_id` supplied to the `triggerworkflow` or the `looply_execution_id` if you did not supply one. |

#### Headers

| Name                                        | Type    | Description    |
| ------------------------------------------- | ------- | -------------- |
| x-api-key<mark style="color:red;">\*</mark> | string  | Looply API Key |

#### Request Body

| Name                                             | Type   | Description                                         |
| ------------------------------------------------ | ------ | --------------------------------------------------- |
| payload<mark style="color:red;">\*</mark>        | object | Payload for workflow                                |
| payload.source<mark style="color:red;">\*</mark> | string | Source of request - must be either `SAP` or `TEAMS` |

#### Example Body

```json
{
    "payload": {
        "source":"SAP", // or "TEAMS" -  Source is a protected attribute
        // Any other relevant data for your workflow here...
    }
}
```

{% tabs %}
{% tab title="200 OK" %}
```json
{
  "message": "success", 
  "code": "WORKFLOW_RESTARTED",
  "data": {
    "workflow_identifier": {
      "organization_id": "org-123",
      "workflow_id": "workflow-123",
      "workflow_execution_id": "execution-123"
    }
  }
}
```
{% endtab %}

{% tab title="404 Not Found " %}
A workflow execution could not be found with the specified process ID for your organization.&#x20;

**EXECUTION\_NOT\_FOUND**

```json
{
  "code": "EXECUTION_NOT_FOUND",
  "data": {
    "organization_id": "org-123"
  }
}
```
{% endtab %}

{% tab title="403 Forbidden" %}
The API key provided is missing or invalid and access has been denied

```json
{
    "message": "Forbidden"
}
```
{% endtab %}

{% tab title="409 Conflict" %}
Workflow execution could not be resumed due to a conflict.&#x20;

**EXECUTION\_IN\_PROGRESS**

An execution with this process ID is in progress and cannot be resumed right now - this may be that your workflow execution has not yet reached it's wait state or moved past it.&#x20;

```json
{
  "code": "EXECUTION_IN_PROGRESS",
  "data": {
    "execution_id": "execution-123",
    "workflow_id": "workflow-123",
    "workflow_status": "IN_PROGRESS",
    "workflow_current_state": "some-state",
    "workflow_process_id": "process-123",
    "execution_description": "description",
    "timestamp": "2023-06-15T12:34:56.789Z"
  }
}
```

**NO\_RESUMABLE\_EXECUTION**

All workflow executions with this process ID have completed and cannot be resumed.

```json
{
  "code": "NO_RESUMABLE_EXECUTION",
  "data": {
    "execution_key": "execution-123"
  }
}
```

**ALREADY\_RESTARTED**

This workflow execution has just recently restarted with the provided resume token.&#x20;

```json
{
  "code": "ALREADY_RESTARTED",
  "data": {
    "execution_key": "execution-123"
  }
}
```
{% endtab %}

{% tab title="500 Internal Server Error" %}
A server error has occurred preventing this workflow from being resumed. Contact Looply Support.&#x20;

**WORKFLOW\_RESTART\_FAILED**

We were unable to restart your workflow due to a fatal server error.&#x20;

```json
{
  "code": "WORKFLOW_RESTART_FAILED",
  "data": {
    "execution_key": "execution-123"
  }
}
```

**MISSING\_RESUME\_TOKEN**

We were unable to restart your workflow due to an error with your execution resume token.&#x20;

```json
{
  "code": "MISSING_RESUME_TOKEN",
  "data": {
    "execution_key": "execution-123"
  }
}
```
{% endtab %}
{% endtabs %}



## terminateWorkflow

<mark style="color:green;">`POST`</mark> `https://api.looply.io/v2/workflows/terminateWorkflow/{process_id}`

Terminate an ongoing execution of your workflow - supports providing a termination reason.&#x20;

**Path Parameters**

| Name                                          | Type   | Description                                                                                           |
| --------------------------------------------- | ------ | ----------------------------------------------------------------------------------------------------- |
| process\_id<mark style="color:red;">\*</mark> | string | `process_id` supplied to the `triggerworkflow`or the `looply_execution_id` if you did not supply one. |

**Headers**

| Name                                        | Type    | Description    |
| ------------------------------------------- | ------- | -------------- |
| x-api-key<mark style="color:red;">\*</mark> | string  | Looply API Key |

#### Request Body

<table><thead><tr><th width="264">Name</th><th>Type</th><th>Description</th></tr></thead><tbody><tr><td>payload<mark style="color:red;">*</mark></td><td>object</td><td>Payload for workflow</td></tr><tr><td>payload.termination_reason</td><td>string</td><td>Optional reason for termination - defaults to <code>"Workflow execution terminated by user."</code> </td></tr></tbody></table>

**Example Body**

```json
{
    "payload": {
        "termination_reason": "Process has now expired."
    }
}
```



{% tabs %}
{% tab title="200" %}
```json
{
    "message": "terminated"
}
```
{% endtab %}

{% tab title="404" %}
```json
{
    // Execution or process ID supplied in path is incorrect
    // or exection is not actively running
    "message": "execution not found"
}
```
{% endtab %}
{% endtabs %}

## toggleScheduledWorkflow

<mark style="color:green;">`POST`</mark> `https://api.looply.io/v2/workflows/toggleScheduledWorkflow`

Pause and resume scheduled workflows.

#### Headers

| Name      | Type   | Description    |
| --------- | ------ | -------------- |
| x-api-key | string | Looply API Key |

#### Request Body

| Name              | Type   | Description             |
| ----------------- | ------ | ----------------------- |
| workflow\_id      | String | id of the workflow      |
| workflow\_version | Number | version of the workflow |
| action            | String | `PAUSE` or `PLAY`       |

{% tabs %}
{% tab title="200: OK Success" %}
Example Payload:

```json
{
	"message": "success",
	"workflow_id": "be9439c2-16b9-4de2-bb1a-9154b85af065",
	"workflow_version": 1,
	"status": "ACTIVE" // or "PAUSED"
}
```
{% endtab %}

{% tab title="400: Bad Request Incorrect input body provided" %}

{% endtab %}

{% tab title="409: Conflict Wrong Action Provided." %}

{% endtab %}

{% tab title="500: Internal Server Error Server side error." %}

{% endtab %}
{% endtabs %}

## getWorkflowExecutionById

<mark style="color:blue;">`GET`</mark> `https://api.looply.io/v2/workflows/getWorkflowExecutionById`

Returns the current state and logs for the workflow execution.

#### Query Parameters

| Name                                                      | Type   | Description                   |
| --------------------------------------------------------- | ------ | ----------------------------- |
| workflow\_id<mark style="color:red;">\*</mark>            | String | Id of the workflow            |
| workflow\_execution\_id<mark style="color:red;">\*</mark> | String | Id for the workflow execution |

#### Headers

| Name                                        | Type    | Description    |
| ------------------------------------------- | ------- | -------------- |
| x-api-key<mark style="color:red;">\*</mark> | String  | Looply API Key |

{% tabs %}
{% tab title="200: OK Success" %}
Example Payload:

```json
{
	"message": "success",
	"item": {
		"workflow_current_state": "State Name",
		"organization_id": "****",
		"workflow_execution_id": "****",
		"modified_by": null,
		"workflow_version": 1,
		"workflow_event_log": {
			// trimmed down logs
		},
		"workflow_id": "****",
		"workflow_input_payload": {
			// combined input payload
		},
		"workflow_output_payload": {
			// combined output payload
		},
		"workflow_trigger_type": "REQUEST",
		"workspace_id": "****",
		"created_on": 1697641835712,
		"condensed_logs": {
            		// input/output for each step. Condensed
        	},
		"workflow_details": { 
			// structure of the workflow - connections and steps
		}
    }
```
{% endtab %}

{% tab title="400: Bad Request Incorrect or missing Query Parameters" %}

{% endtab %}

{% tab title="404: Not Found Execution does not exist" %}

{% endtab %}

{% tab title="500: Internal Server Error Server side Error" %}

{% endtab %}

{% tab title="401: Unauthorized You don't have access to this execution." %}

{% endtab %}

{% tab title="403: Forbidden Incorrect or No API Key Supplied" %}

{% endtab %}
{% endtabs %}

## getWorkflowExecutionHistory

<mark style="color:blue;">`GET`</mark> `https://api.looply.io/v2/workflows/getWorkflowExecutionHistory`

Returns a list of workflow executions. Will return a `lastKey` attribute. Use this to get the next page of data.&#x20;

Limit can be added to the request, 30 is the max.

#### Query Parameters

| Name                                           | Type   | Description                                          |
| ---------------------------------------------- | ------ | ---------------------------------------------------- |
| workflow\_id<mark style="color:red;">\*</mark> | string | id of the workflow                                   |
| limit                                          | string | Number of items to return per page. Defaults to `20` |
| workflow\_execution\_id                        | string | Pagination Key from the `lastKey` attribute.         |

#### Headers

| Name                                        | Type   | Description    |
| ------------------------------------------- | ------ | -------------- |
| x-api-key<mark style="color:red;">\*</mark> | string | Looply API Key |

{% tabs %}
{% tab title="200: OK Success" %}
Example Payload:

```json
{
	"message": "success",
	"count": 20,
	"items": [
		{
			"workflow_id": "****",
			"workflow_current_state": "wait_for_response_state",
			"modified_on": 1697454831391,
			"organization_id": "****",
			"workflow_execution_id": "****",
			"modified_by": null,
			"workflow_version": 1,
			"workflow_event_log": {
				// trimmed down logs
			},
			"workflow_trigger_type": "REQUEST",
			"workspace_id": "****",
			"created_on": 1697454828765,
			"workflow_details": {
				"workflow_name": "Workflow name"
			}
		}
		
	],
	"lastKey": {
		"workflow_id": "****",
		"workflow_execution_id": "****"
	}
}
```
{% endtab %}

{% tab title="400: Bad Request Incorrect or missing Query Parameters" %}

{% endtab %}

{% tab title="401: Unauthorized You don't have access to this workflow's executions." %}

{% endtab %}

{% tab title="403: Forbidden Incorrect or No API Key Supplied" %}

{% endtab %}

{% tab title="500: Internal Server Error Server side error." %}

{% endtab %}
{% endtabs %}

## getOrganizationExecutions

<mark style="color:blue;">`GET`</mark> `https://api.looply.io/v2/workflows/getOrganizationExecutions`

Returns a list of all the workflow executions for an organization. Will return a `lastKey` attribute. Use this to get the next page of data.&#x20;

Limit can be added to the request, 30 is the max.

#### Query Parameters

| Name                    | Type   | Description                                          |
| ----------------------- | ------ | ---------------------------------------------------- |
| limit                   | string | Number of items to return per page. Defaults to `20` |
| workflow\_id            | string | Pagination Key from the `lastKey` attribute          |
| workflow\_execution\_id | string | Pagination Key from the `lastKey` attribute          |

{% tabs %}
{% tab title="200: OK Success" %}
Example Payload:

```json
{
	"message": "success",
	"count": 20,
	"items": [
		{
			"workflow_id": "****",
			"workflow_current_state": "wait_for_response_state",
			"modified_on": 1697454831391,
			"organization_id": "****",
			"workflow_execution_id": "****",
			"modified_by": null,
			"workflow_version": 1,
			"workflow_event_log": {
				// event log
			},
			"workflow_trigger_type": "REQUEST",
			"workspace_id": "****",
			"created_on": 1697454828765,
			"workflow_details": {
				"workflow_name": "Workflow name"
			}
		},
		
	],
	"lastKey": {
		"workflow_id": "****",
		"workflow_execution_id": "****"
	}
}
```
{% endtab %}

{% tab title="403: Forbidden Incorrect or No API Key Supplied" %}

{% endtab %}

{% tab title="500: Internal Server Error Server side error." %}

{% endtab %}
{% endtabs %}

## getWorkflowById

<mark style="color:blue;">`GET`</mark> `https://api.looply.io/v2/workflows/getWorkflowById`

Returns all data for a Looply Workflow

#### Query Parameters

| Name                                                | Type   | Description             |
| --------------------------------------------------- | ------ | ----------------------- |
| workflow\_id<mark style="color:red;">\*</mark>      | string | Id for the workflow     |
| workflow\_version<mark style="color:red;">\*</mark> | string | Version of the workflow |

#### Headers

| Name                                        | Type    | Description    |
| ------------------------------------------- | ------- | -------------- |
| x-api-key<mark style="color:red;">\*</mark> | string  | Looply API Key |

{% tabs %}
{% tab title="200: OK Success" %}
Example Payload:

```json
{
  "message": "success",
  "item": {
    "organization_id": "****",
    "workflow_integrations": {
      // integrations configured for this workflow - e.g. SAP profile
    },
    "workflow_name": "****",
    "created_by": "test-user",
    "workflow_status_version": "DRAFT_1",
    "modified_by": null,
    "workflow_version": 1,
    "workflow_steps": [
      // states within the workflow
    ],
    "workflow_id": "****",
    "modified_on": 1697642922168,
    "workflow_connections": [
      // connections between all the workflow states
    ],
    "workflow_status": "DRAFT",
    "latest": "true",
    "workspace_id": "****",
    "created_on": 1695136500202,
    "workflow_schema": {
      // workflow schema
    }
  }
}

```
{% endtab %}

{% tab title="400: Bad Request Incorrect or missing Query Parameters" %}

{% endtab %}

{% tab title="401: Unauthorized You don't have access to this workflow." %}

{% endtab %}

{% tab title="403: Forbidden Incorrect or No API Key Supplied" %}

{% endtab %}

{% tab title="404: Not Found Execution does not exist" %}

{% endtab %}

{% tab title="500: Internal Server Error Server side Error" %}

{% endtab %}
{% endtabs %}

## getOrganizationWorkflows

<mark style="color:blue;">`GET`</mark> `https://api.looply.io/v2/workflows/getOrganizationWorkflows`

Returns a list organization workflows. Will return a `lastKey` attribute. Use this to get the next page of data.&#x20;

Limit can be added to the request, 30 is the max.

#### Query Parameters

| Name              | Type   | Description                                    |
| ----------------- | ------ | ---------------------------------------------- |
| limit             | string | Number of items to return per page. Default 20 |
| workflow\_id      | string | Pagination Key from the `lastKey` attribute.   |
| workflow\_version | string | Pagination Key from the `lastKey` attribute.   |

#### Headers

| Name                                        | Type   | Description    |
| ------------------------------------------- | ------ | -------------- |
| x-api-key<mark style="color:red;">\*</mark> | string | Looply API Key |

{% tabs %}
{% tab title="200: OK Success" %}
Example Payload:

```json
{
  "message": "success",
  "count": 20,
  "items": [
    {
      "workflow_id": "****",
      "modified_on": 1693488718083,
      "organization_id": "****",
      "workflow_status": "ACTIVE",
      "latest": "false",
      "workflow_name": "Adaptive Card Workflow",
      "created_by": "****",
      "workflow_status_version": "ACTIVE_3",
      "modified_by": "****",
      "workspace_id": "****",
      "created_on": 1692614072550
    }
  ],
  "lastKey": {
    "organization_id": "****",
    "workflow_id": "****",
    "workflow_version": 1
  }
}
```
{% endtab %}

{% tab title="500: Internal Server Error Server side Error" %}

{% endtab %}
{% endtabs %}

## getWorkflowSchemaById

<mark style="color:blue;">`GET`</mark> `https://api.looply.io/v2/workflows/getWorkflowSchemaById`

Returns the data schema for a workflow

#### Query Parameters

| Name                                                | Type   | Description             |
| --------------------------------------------------- | ------ | ----------------------- |
| workflow\_id<mark style="color:red;">\*</mark>      | string | Id of the workflow      |
| workflow\_version<mark style="color:red;">\*</mark> | string | Version of the workflow |

#### Headers

| Name                                        | Type   | Description    |
| ------------------------------------------- | ------ | -------------- |
| x-api-key<mark style="color:red;">\*</mark> | string | Looply API Key |

{% tabs %}
{% tab title="200: OK Success" %}
Example Payload:

```json
{
  "message": "success",
  "item": {
    "workflow_id": "****",
    "modified_on": 1697642922168,
    "created_by": "test-user",
    "workflow_mock_payload": {
      // mock payload from a previous execution
    },
    "workflow_schema": {
      // schema for the mock payload
    },
    "modified_by": null,
    "workflow_version": 1,
    "created_on": 1695136500202
  }
}

```
{% endtab %}

{% tab title="400: Bad Request Incorrect or missing Query Parameters" %}

{% endtab %}

{% tab title="401: Unauthorized You don't have access to this execution." %}

{% endtab %}

{% tab title="403: Forbidden Incorrect or No API Key Supplied" %}

{% endtab %}

{% tab title="404: Not Found Execution does not exist" %}

{% endtab %}

{% tab title="500: Internal Server Error Server side Error" %}

{% endtab %}
{% endtabs %}

## getAppUserInstallationStatus

<mark style="color:blue;">`GET`</mark> `https://api.looply.io/v2/workflows/getAppUserInstallationStatus`

Returns information for if the specified user has an app installed within your organisation

#### Query Parameters

| Name                                      | Type   | Description           |
| ----------------------------------------- | ------ | --------------------- |
| email<mark style="color:red;">\*</mark>   | string | Email address of user |
| app\_id<mark style="color:red;">\*</mark> | string | ID of the app         |

#### Headers

| Name                                        | Type   | Description    |
| ------------------------------------------- | ------ | -------------- |
| x-api-key<mark style="color:red;">\*</mark> | string | Looply API Key |

{% tabs %}
{% tab title="200: OK Success" %}
Example Payload:

```json
{
  "message": "success",
  "email": "user@organization.com", 
  "app_id": "1234", 
  "installed": "true"
}
```
{% endtab %}

{% tab title="400: Bad Request Incorrect or missing Query Parameters" %}
Example Response

```json
{
  "message": "Missing required parameters for this request."
}
```
{% endtab %}

{% tab title="403: Forbidden Incorrect or No API Key Supplied" %}
Example Response

```json
{
  "message": "Forbidden"
}
```
{% endtab %}

{% tab title="500: Internal Server Error Server side Error" %}
Example Response

```json
{
  "message": "something went wrong"
}
```
{% endtab %}
{% endtabs %}
