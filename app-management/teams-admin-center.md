---
description: >-
  Guide to Pre-Install an Looply App via Teams Admin Center to organizational
  users
---

# Teams Admin center

## **Prerequisites**

**Admin Access:** You must have admin permissions to access the Microsoft Teams Admin Center.

**Looply App:** Ensure the Looply app is already listed under Teams apps > Manage apps. ([Refer to deploying apps to App catalog section](deploying-apps-to-teams-app-catalog/))

## **Steps to Pre-Install the App**

1. **Log in to the Teams Admin Center**
   * Visit the Teams Admin Center ([https://admin.teams.microsoft.com](https://admin.teams.microsoft.com)).
   * Sign in using your Microsoft 365 admin credentials.

<figure><img src="../.gitbook/assets/Screenshot 2024-11-28 at 18.03.30.png" alt=""><figcaption></figcaption></figure>

1. **Navigate to App Setup Policies**
   * In the left navigation pane, go to **Teams apps > Setup policies.**
   * Review the available setup policies or create a new one.
2. **Add the App to a Setup Policy**
   * Choose an existing policy to modify or click Add to create a new one.
   * Under the Installed apps section:
     * Click Add apps.
     * Search for the Looply app by name (eg: Task order).
     * Select the app and click Add.
   * (Optional) Under the Pinned apps section:
     * Click Add apps.
     * Search for the app by name and click Add to make it appear in the Teams navigation bar.
     * Adjust the order of pinned apps using the Move up or Move down buttons.
3. **Assign the Policy to Users**
   * Go to Users > Manage users.
   * Search for the user or group of users to whom you want to assign the policy.
   * Click the user’s name, and under Policies, click Edit.
   * Assign the desired app setup policy.&#x20;
   * Bulk Assignment:&#x20;
     * To assign the policy to multiple users, go to Teams apps > Setup policies, select the policy, and click Assign users.&#x20;
     * Upload a CSV file containing the list of user email addresses or manually select users.
   * (Alternatively) go to **Teams apps > Setup policies >** Manager users > Assign to required teams users&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2024-11-28 at 18.05.00.png" alt=""><figcaption></figcaption></figure>

1. **Verify Deployment**
   1. Ask a test user to log into Teams and verify that the app is pre-installed and, if pinned, visible in  the navigation bar.
   2. &#x20;You can check the app's status in Teams apps > Manage apps or through user activity logs.

***

## FAQs

1\) How long does it take for the app to appear?

It may take a few hours for the policy changes to propagate to all assigned users.

2\) What if the app is not visible?

·       Ensure the app is published in the Manage apps section.

·       Verify that the setup policy is correctly assigned to the user or group.

·       Get users to signout of teams and sign back in

3\) Can I enforce app usage?

Pre-installation does not enforce usage, but pinning the app ensures high visibility.
