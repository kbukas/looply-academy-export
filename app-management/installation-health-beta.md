# Installation Health (Beta)

The Installation Health tab helps administrators verify whether installed Teams apps can actually deliver messages to users. An app may appear "installed" in Microsoft but still fail to deliver messages for several reasons — the user hasn't opened Teams, the conversation has gone stale, or the user has blocked the bot. Installation Health performs deep checks to uncover these issues.

{% hint style="info" %}
**Note:** Installation Health is currently in Beta. Access it from **App Manager → select your app →**&#x20;

**Installation Health** tab.
{% endhint %}

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

### Health Statuses

After running a health check, each user displays one of the following statuses:

| Status            | Colour | What it means                                                                 | What to do                                            |
| ----------------- | ------ | ----------------------------------------------------------------------------- | ----------------------------------------------------- |
| **Healthy**       | Green  | Everything is working. Messages will be delivered.                            | No action needed.                                     |
| **Recovered**     | Green  | A connection issue was detected and automatically fixed.                      | No action needed. Messages can now be delivered.      |
| **Provisioning**  | Amber  | The app is installed but the user hasn't opened Teams yet.                    | Ask the user to open Teams and wait a few minutes.    |
| **No Context**    | Red    | Looply is missing the connection details needed to send messages.             | Uninstall and reinstall the app for this user.        |
| **Stale Context** | Red    | The connection details are outdated and could not be automatically recovered. | Uninstall and reinstall the app for this user.        |
| **Blocked**       | Red    | The user has blocked the bot in Teams.                                        | The user must unblock it — see troubleshooting below. |
| **Not Installed** | Red    | The app is no longer installed for this user.                                 | Reinstall the app.                                    |
| **Failed**        | Red    | The health check encountered an error.                                        | Try running the check again.                          |
| **Not Checked**   | Grey   | No health check has been run yet.                                             | Click the health check button to check this user.     |

### User Status

The User Status column shows the user's current Teams presence — Available, Busy, Away, Offline, etc. This is informational only and does not affect the health status. It can help determine whether the user is actively using Teams.

### Actions

Each user row provides the following actions:

#### Profile Sync

Fetches the latest user profile from Microsoft and updates the local record — including display name, email, and job title.

**When to use:**

* A user's name or email has changed but Looply still shows the old details
* The email column appears blank or outdated
* After organisational changes such as name changes or email migrations

{% hint style="info" %}
**Tip:** Profile data is not automatically refreshed. Click the sync button to pull the latest details. The table updates immediately after a successful sync.
{% endhint %}

***

#### Check Health

Runs a comprehensive check to verify whether the app installation is fully functional and messages can be delivered.

**When to use:**

* After installing the app for a user, to confirm it completed successfully
* When a user reports they are not receiving messages
* After a user re-installs the app or changes devices
* To periodically audit installation health across your organisation

{% hint style="info" %}
**Tip:** If the status returns **Provisioning**, ask the user to open Microsoft Teams and wait a few minutes before checking again. If the status is **No Context** or **Stale Context**, uninstall and reinstall the app for that user.
{% endhint %}

***

#### Send Test Message

Sends a test card to the user's Teams chat to verify end-to-end message delivery.

**When to use:**

* After a health check returns **Healthy**, to confirm actual delivery
* When a user reports missing notifications despite a healthy status
* After resolving a blocked bot or completing a reinstallation

**Limits:**

* Only one test message can exist per user at a time
* If delivery fails, a 60-second cooldown applies before retrying
* The error notification will indicate the specific reason for failure

Once a test message is successfully sent, a **delete button** appears next to the send button. Use it to remove the test message from the user's chat after confirming delivery. The delete button remains available across page refreshes until the message is deleted.

{% hint style="info" %}
**Tip:** Delete the test message after confirming delivery to avoid confusing end users.
{% endhint %}

### Searching

Use the search field at the top of the tab to find users by name or email. The search covers all users in your organisation, not just those on the current page.

### Last Health Check

This column shows when the most recent action was performed, displayed as relative time (e.g., "2m ago", "3h ago", "Yesterday"). Hover to see the exact timestamp. The type of action is shown below — such as "Health check", "Test message", "Profile sync", or "Delete message".

### Troubleshooting&#x20;

| Symptom                             | Likely Status                      | Resolution                                                                                                                 |
| ----------------------------------- | ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| User not receiving messages         | **Provisioning**                   | Ask the user to open Microsoft Teams and wait a few minutes                                                                |
| User not receiving messages         | **No Context** / **Stale Context** | Uninstall and reinstall the app for this user                                                                              |
| User not receiving messages         | **Blocked**                        | User must unblock the bot: **Teams → Chat → find the bot conversation → right-click → Unblock**                            |
| User not receiving messages         | **Not Installed**                  | Reinstall the app from the User Manager tab                                                                                |
| User not receiving messages         | **Healthy**                        | Send a **Test Message** to confirm delivery — if it fails, check the error for the specific reason                         |
| Name or email is incorrect          | —                                  | Click **Profile Sync** to fetch the latest details from Microsoft                                                          |
| App reinstalled but still unhealthy | **No Context**                     | Wait 1–2 minutes, run a health check again. If still unhealthy, ask the user to open Teams and send any message to the bot |
| Test message not arriving           | Any non-Healthy                    | Resolve the health status first, then retry the test message                                                               |
| Health check returns **Failed**     | **Failed**                         | This is usually a temporary issue. Wait a moment and try again                                                             |
