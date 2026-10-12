---
description: Configure email or webhook notifications if your workflow fails in Looply
---

# Error Notifications

In modern workflow automation, ensuring that each step of a process is executed correctly is crucial. However, errors can sometimes occur, and timely notifications are essential to promptly address these issues and maintain workflow integrity. Looply provides robust error notification features that allow users to configure alerts to be sent via email or to a specified webhook whenever a step in their workflow fails. This ensures that users are immediately informed of any issues, enabling quick troubleshooting and minimal disruption to their automated processes.

## Configuring Error Notifications

Looply offers flexible and customisable options for error notifications, tailored to meet the unique needs of each version of your workflows. You can choose to receive notifications directly to your email inbox or have them sent to a webhook, which can be integrated with your existing monitoring and alerting systems.

<figure><img src="../.gitbook/assets/Screenshot 2025-06-24 at 10.24.41.png" alt=""><figcaption><p>Workflow - Error Notifications</p></figcaption></figure>

### Email Notifications

Email notifications in Looply provide a straightforward way to stay informed about errors in your workflows. You can configure email alerts to be sent every time an error occurs, ensuring immediate awareness and quick response to any issues. Notifications can be sent to one or multiple recipients of your choosing, allowing you to keep all relevant team members informed.

To prevent email spam and manage the frequency of notifications, Looply also allows you to set a preferred frequency for receiving error alerts. You can choose to receive notifications every hour, day, week, or month, depending on your needs. This flexibility ensures that you stay informed without being overwhelmed by too many emails.

Email notifications can be configured by entering a **Frequency** and one or more **Recipients** from within the Workflow Settings menu of your selected workflow version. &#x20;

### Webhook Notifications

Webhook notifications in Looply offer a powerful and flexible way to integrate error alerts with your existing systems. You can configure webhook notifications alongside email notifications, enabling you to use both methods concurrently. This ensures you have multiple channels of communication, enhancing your ability to respond promptly to any workflow errors.

By setting up a webhook URL, Looply will send a POST request with detailed error information to the specified endpoint every time a step fails. This allows you to send error notifications directly to your own system, where you can handle them appropriately based on your unique requirements.

Looply provides the following configuration options for webhook notifications:

* **Webhook URL**: Enter the URL to which Looply will send the error notifications.
* **Custom Headers**: Specify any custom headers that need to be included in the notification request - such as an authorisation token or API key.
* **Basic Authentication**: If accessing an authenticated URL, you can enter a username and password to enable basic authentication.

Once a webhook URL has been configured, Looply will automatically begin attempting to send POST requests with any information whenever an error occurs within your workflow.&#x20;

#### Example Webhook Notification Payload

```json
{
    "workflow_id": "02b2de0a-9a01-4e08-9844-4d9b8e3b37bc",
    "workflow_execution_id": "f39462a9-8b42-4d60-8a74-ca75b0a482cb",
    "organization_id": "2c3662ac-4367-547c-d1be-9f48fba22e63",
    "error": {
        "step": "My Function", 
        "name": "ReferenceError", 
        "message": "x is not defined"
    }
}
```

### Conditional Notifications

Error notifications can be enabled to be sent out conditionally based on workflow variables which can give you fine-grained control over your notification strategy, ensuring that your team only receives alerts when specific conditions are met.

#### How It Works

When enabled, error notifications (both email and webhook) will only be sent if the specified workflow variable evaluates to a truthy value. This allows you to implement custom logic to determine when notifications should be triggered.

#### Configuration

1. In the workflow settings panel, navigate to the **Error Notifications** section
2. Toggle the **Enable conditional notifications** switch to activate this feature
3. In the **Condition** field, specify a workflow variable that will control notification delivery
   * The variable must evaluate to a truthy value for notifications to be sent
   * If the variable is falsy (false, 0, null, undefined, empty string), no notifications will be sent

We support condition variables being selected from your incoming workflow payload, any imported datastores or your workflow environment variables.&#x20;
