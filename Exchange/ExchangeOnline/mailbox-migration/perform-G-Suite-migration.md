---
title: "Perform a Google Workspace migration"
ms.author: jhendr
author: msdmaguire
manager: serdars
audience: Admin
ms.topic: conceptual
ms.service: exchange-online
localization_priority: Normal
search.appverid: MET150
f1.keywords:
- NOCSH
description: Instructions for migrating from Google Workspace to Microsoft 365 or Office 365 in stages by migrating users in batches.
ms.custom: seo-marvel-apr2020
---

# Perform a Google Workspace (formerly G Suite) migration

You can migrate batches of users from Google Workspace to Microsoft 365 or Office 365, allowing a migration project to be done in stages. This migration requires that you provision all of your users who will be migrated as mail-enabled users outside of the migration process. You must specify a list of users to migrate for each batch. By default, we migrate mail, calendar, contacts, and rules for all users listed in the batch. 

All of the procedures in this article assume that your Microsoft 365 or Office 365 domain has already been verified and your TXT records have been set up. For more information, see [Set up your domain (host-specific instructions)](/microsoft-365/admin/get-help-with-domains/set-up-your-domain-host-specific-instructions).

   > [!NOTE]
   > Google Workspace migration is not currently available for Office 365 US Government GCC High or DoD.

## Overview of the process

Before beginning your migration, review the following diagrams to understand how a Google Workspace staged migration works. The diagrams show how a fictitious company named Fabrikam, Inc., with the domain name *fabrikaminc.net* performed their migration.

![Original setup before G Suite migration](../media/gsuite-mig-original-setup.png)

Prior to their migration, the MX record for the base "fabrikaminc.net" domain points to the Google Workspace tenant or mail server where all or most of Fabrikam, Inc.'s users are. Note that users have their primary email addresses at that domain.

![Before the G Suite migration begins](../media/gsuite-mig-before-migration.png)

The MX record for the primary domain "fabrikaminc.net" still points to Google Workspace, where all the primary mailboxes reside. To prepare for the migration, new routing domains have been created: the *gsuite.fabrikaminc.net* domain points to Google Workspace and the *o365.fabrikaminc.net* domain points to Microsoft 365 or Office 365.

On the Google Workspace side, aliases have been added for all of the users in the Google Workspace routing domain. On the Microsoft 365 or Office 365 side, MailUsers have been provisioned for all of the users from the Google Workspace tenant. The ExternalEmailAddress field for MailUsers on the Microsoft 365 or Office 365 side were configured to point back to the primary mailbox using the address at the routing domain for the Google Workspace side. Additionally, there should be aliases for the user in the Microsoft 365 or Office 365 routing domain.

The green arrow indicates how, at this point in the migration, User 2 still contacts User 1 through their Google Workspace email addresses.

![During a single batch of a G Suite migration](../media/gsuite-mig-during-batch.png)

User 1 and User 2 are part of the first migration batch to Microsoft 365 or Office 365, while User 3 and User 4 will be part of a later batch. The MX record for the primary domain "fabrikaminc.net" still points to Google Workspace, where all the primary mailboxes still reside. Because User 1 and User 2 have had their migrations started, they've been converted from MailUsers to Mailboxes on the Microsoft 365 or Office 365 side.

The ExternalEmailAddress for each user has been moved to a ForwardingSmtpAddress, so that messages sent to User 1 and User 2 will be delivered back to their source mailboxes on the Google Workspace side by rerouting the message back to the Google Workspace routing domain. This is indicated by the red arrows in the above diagram. Mail is still being synced from the source Google Workspace side to the Microsoft 365 or Office 365 side.

![After a single batch of a G Suite migration](../media/gsuite-mig-after-batch.png)

The MX record for the primary domain "fabrikaminc.net" still points to Google Workspace. Now that User 1 and User 2 have been fully migrated to Microsoft 365 or Office 365, they should start working out of Microsoft 365 or Office 365. On the Google Workspace side, automatic mail forwarding has been set up for migrated users, so that new emails sent to their Google Workspace address will be delivered instead to the Microsoft 365 or Office 365 address via the routing domain. This is shown by the green arrows in the above diagram.

> [!IMPORTANT]
> If your organization has disabled a user's ability to set a forwarding address, the Google Workspace migration tool will also be unable to set the forwarding address. You must enable permissions to set SMTP forwarding in order for forwarding to be set successfully during your migration. For more information, see [Configure outbound spam filtering in EOP](/microsoft-365/security/office-365-security/configure-the-outbound-spam-policy).

Meanwhile, the forwarding address has been removed from the Microsoft 365 or Office 365 user object, so emails will be delivered to that user in the Microsoft 365 or Office 365 routing domain (as shown by the red arrows above).

![After G Suite migration is complete](../media/gsuite-mig-after-migration.png)

After all migration batches have been completed, all users can use their migrated mailboxes on Microsoft 365 or Office 365 as their primary mailbox. A manual MX record update for the primary domain "fabrikaminc.net" then points to the Microsoft 365 or Office 365 organization instead of the Google Workspace tenant. The routing domains and extra aliases can now be removed, as can the Google Workspace tenant. The migration of mail, calendar, contacts, and rules from Google Workspace to Microsoft 365 or Office 365 is now complete.

## Migration limitations

> [!NOTE]
> The largest single email message that can be migrated is based on the transport configuration for your configuration. The default limit is 35 MB. To increase this limit, see [Office 365 now supports larger email messages](https://www.microsoft.com/en-us/microsoft-365/blog/2015/04/15/office-365-now-supports-larger-email-messages-up-to-150-mb/).

Throughput limitations for contacts and calendars completely depend on the quota restrictions for your tenant's service account on the Google Workspace side.

Other migration limitations are described in the following table:

<br>

****

|Data type|Limitations|
|---|---|
|Mail|Vacation settings, Automatic reply settings|
|Meeting rooms|Room bookings will not be migrated|
|Calendar|Shared calendars, cloud attachments, Google Hangout links, and event colors will not be migrated|
|Contacts|A maximum of three email addresses per contact are migrated over|
|Contacts|Gmail tags, contact URLs, and custom tags will not be migrated|
|

> [!TIP]
> If you will be [starting your migration batch with Exchange Online Powershell](#start-a-google-workspace-migration-with-exchange-online-powershell), as described later in this article, you can use the `-ExcludeFolder` parameter to prevent certain folders from being migrated. This will reduce the amount of data in your migration, as well as the size of a user's new Exchange Online mailbox. You can identify folders you don't want to migrate by name, and you can also identify Gmail labels that apply to multiple messages in order to exclude those messages from the migration. For more information on using `-ExcludeFolder`, see [New-MigrationBatch](/powershell/module/exchange/new-migrationbatch).
>
> If you wish to skip the migration of Gmail filters, you can use the `-SkipRules` parameter to prevent the migration of Outlook rules. For more information on using `-SkipRules`, see [New-MigrationBatch](/powershell/module/exchange/new-migrationbatch). 

## Create a Google Service Account

> [!IMPORTANT]
> Use Chrome to create your Google Service account. Other browsers may not allow you to do this properly. <br/><br/> Because elements of the Google Workspace user interface can change over time, the screens you see might vary from the examples in this section. The locations of certain fields may vary as well. Please look at Google's Documentation for how to [Create a Service Account](https://support.google.com/a/answer/7378726) for clarifications in case the UI has changed significantly.

1. In Chrome, go to the [Google Cloud Platform](https://console.developers.google.com/iam-admin/serviceaccounts) and sign in as a Google user (such as the Google Workspace admin).

2. Click **Create** to create and name a new project for the onboarding (such as "Google Workspace project"), or click **Select** to select an existing project.

   ![Create a project](../media/new-project-onboarding-im1.png)

3. Navigate to **IAM & Admin** > **Service Accounts** > **Select Project** and then click **Create Service Account** and, in **Service account name**, give the service account a name, such as "Google Workspace Onboarding." Click **Create**.

   ![Create service account](../media/create-service-account-im2.png)

4. On the **Service account permissions (optional)** screen, click **Continue**.

5. On the **Grant users access to this service account (optional)** screen, click **Done**.

6. Once you have been returned to the page listing the service accounts, click on the **Email** for the Service Account you just created to enter the details page, then click the **Edit** button. Alternatively, you can click the ellipsis under the **Actions** column and select the **Edit** action.

7. On the Service account details page, note the **Unique ID**. This is the ClientId that you will provide later in the instructions for [Grant access to the service account for your Google tenant](#grant-access-to-the-service-account-for-your-google-tenant).

   ![Unique ID](../media/service-account-details-im3.png)

8. If you see an area that says **Show Domain-Wide Delegation**, click to expand that section. Next, enable the checkbox to **Enable G Suite Domain-wide Delegation**, enter the product name to configure the consent screen, and then click **Save** at the bottom of the page.

   ![Domain-Wide Delegation](../media/service-account-status-im4.png)

9. On the Service account details page, click **Add Key** and select the **Create new key** option.

   ![Create key](../media/Keys-im5.png)

10. Under **Key type**, make sure **JSON** is selected, and then click **Create**.

    ![JSON key](../media/create-keys-im6.png)

11. Keep track of the JSON key file that is automatically downloaded, as you will need its filename during the steps under [Create a migration endpoint in Microsoft 365 or Office 365](#create-a-migration-endpoint-in-microsoft-365-or-office-365). Close the pop-up dialog and click **Save**.

## Enable API usage in your project

If your project doesn't already have all of the required APIs enabled, you must enable them.

1. Go to the [Developer page for API Library](https://console.developers.google.com/apis/library) and sign in as the Google user you used above in **Create a Google Service Account**.

2. Select the project that you used above.

3. Search for the following APIs; each one must be enabled. Select **Enable** to enable them for your project:
   - Gmail API
   - Google Calendar API
   - People API

   > [!IMPORTANT]
   > The Google Contacts API is deprecated. For more information, see [Google Contacts API](https://developers.google.com/contacts/). </br></br>While transitioning from the Contacts API to the People API, tenants running migrations may see errors indicating "ContactsSyncClientException" with a status of "BadRequest". Simply resuming should allow the migration to continue.

## Grant access to the service account for your Google tenant

1. Go to the [Google Workspace Admin page](https://admin.google.com/AdminHome) and sign in as Google Workspace admin for your tenant.

2. Click **Security**, then click **API Controls**, and then click **Manage Domain Wide Delegation**.

3. Next to the **API Clients** list, click **Add new**.

4. In **Client ID**, type the ClientId for the service account you created in the [Create a Google Service Account](#create-a-google-service-account) section above.

   ![Add new API client](../media/add-a-new-client-id-im7.png)

5. In **OAuth Scopes**, add the required scopes in comma-separated format, with no spaces in between. For example: </br></br> `https://mail.google.com/,https://www.googleapis.com/auth/calendar,https://www.google.com/m8/feeds/,https://www.googleapis.com/auth/gmail.settings.sharing,https://www.googleapis.com/auth/contacts` 

    If the OAuth Scopes are entered incorrectly, the resulting list won't match and the migration process will fail later, after you start the migration batch.

6. Click **Authorize**. Verify that the resulting list shows the expected four (4) OAuth scopes.

   > [!Important]
   > It may take anywhere from 15 minutes to 24 hours for these settings to propogate.

## Create a subdomain for mail routing to Microsoft 365 or Office 365

1. Go to the [Google Workspace Admin page](https://admin.google.com/AdminHome) and sign in as a Google Workspace admin for your tenant.

2. Click **Domains**, then **Manage domains**, and then click **Add a domain**.

   > [!NOTE]
   > The option _Add a domain_ will not be available if using the legacy free edition of G Suite.

3. Enter the domain that you will use for routing mails to Microsoft 365 or Office 365, then click **Continue and verify domain ownership**. A subdomain of your primary domain is recommended (such as "o365.fabrikaminc.net" when "fabrikaminc.net" is your primary domain) so that it will be automatically verified. Keep track of the name of the domain you enter because you will need it for the following steps, and later in the instructions as the Target Delivery Domain when you [Create a migration batch in Microsoft 365 or Office 365](#create-a-migration-batch-in-microsoft-365-or-office-365).

   > [!NOTE]
   > A sub-domain of your primary domain is recommended. If another domain (such as "fabrikaminc.onmicrosoft.com") is set, Google will send emails to each individual address with a link to verify the permission to route mail. Migration won't complete until the verification is completed.

   ![Add domain](../media/add-a-new-domain-im8.png)

4. Follow any subsequent steps that are then required to verify your domain, making sure that the status is shown as **Active**. If you chose a subdomain of your primary domain in step 3 above, your new domain may have been verified automatically.

   ![Verify domain](../media/verify-ownership-im9.png)

5. Log into your DNS provider and update your DNS records so that you have an MX record at the domain you created above in step 3, pointing to Microsoft 365 or Office 365. Ensure that this domain that you created above is an accepted domain in Microsoft 365 or Office 365. Follow the instructions in [Add a domain to Microsoft 365](/microsoft-365/admin/setup/add-domain) to add the Microsoft 365 or Office 365 routing domain ("o365.fabrikaminc.net") to your organization and to configure DNS to route mail to Microsoft 365 or Office 365.

> [!NOTE]
> The migration process won't be able to complete if a routing domain is used that is not verified as described above. Choosing the built-in "tenantname.onmicrosoft.com" domain for routing mail to Office 365 instead of a sub-domain of the primary Google Workspace domain occasionally causes issues that Microsoft is not able to assist with besides to recommend that the user manually verify the forwarding address or contact Google support.

## Create a subdomain for mail routing to your Google Workspace domain

1. Go to the [Google Workspace Admin page](https://admin.google.com/AdminHome) and sign in as a Google Workspace admin for your tenant.

2. Click **Domains**, then **Manage domains**, and then click **Add a domain alias**.

3. Enter the domain that you will use for routing mails to Google Workspace, then click **Continue and verify domain ownership**. A subdomain of your primary domain is recommended (such as "gsuite.fabrikaminc.net" when "fabrikaminc.net" is your primary domain) so that it will be automatically verified.

   ![Add domain alias](../media/add-a-new-domain-alias-im10.png)

4. Follow any subsequent steps that are then required to verify your domain, making sure that the status is shown as **Active**. If you chose a subdomain of your primary domain in step 3 above, your new domain may have been verified automatically.

   ![Verify domain](../media/verify-ownership-im9.png)

5. Follow Google's instructions to [Set up MX records for Google Workspace Gmail](https://support.google.com/a/answer/140034) for this domain.

   > [!NOTE]
   > It may take up to 24 hours for Google to propagate this setting to all of the users in your organization.

   > [!IMPORTANT]
   > If you are using non-default Transport settings in your Microsoft 365 or Office 365 organization, you should check that mail flow will work from Office 365 to Google Workspace. Be sure that either your default Remote Domain ("\*") has Automatic Forwarding enabled, or that there is a new Remote Domain for your Google Workspace routing domain (e.g. "gsuite.fabrikaminc.net") that has Automatic Forwarding enabled.

## Provision users in Microsoft 365 or Office 365

Once your Google Workspace environment has been properly configured, you can complete your migration in the Exchange admin center or through the Exchange Online PowerShell.

Before proceeding with either method, make sure that Mail Users have been provisioned for every user in the organization who will be migrated (either now or eventually). If any users aren't provisioned, provision them using the instructions in [Manage mail users](../recipients-in-exchange-online/manage-mail-users.md).

For more advanced scenarios, you may be able to deploy Azure Active Directory (Azure AD) Connect to provision your Mail Users. See [Deploy Microsoft 365 Directory Synchronization in Microsoft Azure](/office365/enterprise/deploy-office-365-directory-synchronization-dirsync-in-microsoft-azure) for an overview, and [Set up directory synchronization for Microsoft 365](/office365/enterprise/set-up-directory-synchronization) for setup instructions. Then, you need to deploy an Exchange server in your on-premises environment for user management, and mail-enable your users using this server. For more information, see [How and when to decommission your on-premises Exchange servers in a hybrid deployment](../../ExchangeHybrid/decommission-on-premises-exchange.md) and [Manage mail users](/Exchange/ExchangeServer/recipients/mail-users.md). Once the Mail Users have been created in Microsoft 365, the Azure AD Connect may need to be disabled in order to allow the migration process to convert these users into mailboxes - see [Turn off directory synchronization for Microsoft 365](/office365/enterprise/turn-off-directory-synchronization).

We recommend that the primary address (sometimes referred to as the "User ID") for each user be at the primary domain (such as "will@fabrikaminc.net"). Typically, this means that the primary email address should match between Microsoft 365 or Office 365 and Google Workspace. If any user is provisioned with a different domain for their primary address, then that user should at least have a proxy address at the primary domain. Each user should have their `ExternalEmailAddress` point to the user in their Google Workspace routing domain ("will@gsuite.fabrikaminc.net"). The users should also have a proxy address that will be used for routing to their Microsoft 365 or Office 365 routing domain (such as "will@o365.fabrikaminc.net").

> [!NOTE]
> We recommend that the Default MRM Policy and Archive policies be disabled for these users until after their migration has been completed. When such features remain enabled during migration, there is a chance that some messages will end up being considered "missing" during the content verification process.

## Start a Google Workspace migration batch with the new Exchange admin center (New EAC)

> [!IMPORTANT]
> Microsoft's data migration tool is currently unaware of tools enforcing messaging records management (MRM) or archival policies. Because of this, any messages that are deleted or moved to archive by these policies will result in the migration process flagging these items as "missing". The result is perceived data loss rather than actual data loss, which makes it much harder to identify actual data loss during any content verification checks. <br/><br/>Therefore, Microsoft strongly recommends disabling all MRM and archival policies before attempting any data migration to mailboxes.

1. In the new [Exchange Admin center](https://admin.exchange.microsoft.com/#/), navigate to **Migration** > **Batch**.

2. Click **New Migration batch** and follow the instructions in the details pane.

3. In **Migration Onboarding** section, enter the batch name, select the mailbox migration path and click **Next**.

4. Select the migration type as **G Suite (Gmail) migration** from the drop-down list and click **Next**.

   ![Migration Onboarding](../media/migration-onboarding-selection.png)

5. In **G Suite migration prerequisites** section, manually configure G-Suite for migration.

6. In **Set endpoint** section, select the migration endpoint from the drop-down list and click **Next**.

    ![Set endpoint](../media/migration-endpoint-selection.png)

    > [!NOTE]
    > To migrate Gmail mailboxes successfully, Microsoft 365 or Office 365 needs to connect and communicate with Gmail. To do this, Microsoft 365 or Office 365 uses a migration endpoint. Migration endpoint is a technical term that describes the settings that are used to create the connection so you can migrate the mailboxes.

7. Create a CSV file containing the set of all of the users you want to migrate. You will need its filename below. The allowed headers are:

    - EmailAddress (required). Contains the primary email address for an existing Microsoft 365 or Office 365 mailbox.

    - Username (optional). Contains the Gmail primary email address, if it differs from EmailAddress.

      ```CSV
      EmailAddress
      will@fabrikaminc.net
      user123@fabrikaminc.net
      ```

8. In **Add user mailboxes** section, import the CSV file and click **Next**.

9. In **Move configuration** section, enter the details and click **Next**.

10. In **Schedule batch migration** section, verify all the details, click **Save**, and then click **Done**.

    ![Schedule batch migration](../media/schedule-batch1-migration.png)

    Once the batch status changes from **Syncing** to **Synced**, you need to complete the batch.

    > [!NOTE]
    > When the batch starts, all the users to be migrated will be converted from MailUsers to Mailboxes. The Microsoft 365 or Office 365 Exchange license must be assigned only after this moment. You have 30 days to assign the license.

11. To complete the batch, select the migration group.

12. In the details pane, select the preferred option to complete the batch and click **Save**.

    The batch status will then be **Completed**.

During completion, another incremental sync is run to copy any changes that have been made to the Google Workspace mailbox. Additionally, during completion, the forwarding address that routes mail from Microsoft 365 or Office 365 to Google Workspace is removed, and a forwarding address that routes mail from Google Workspace to Microsoft 365 or Office 365 is added. This ensures that any messages received by migrated users at their Google Workspace mailboxes will be sent to their new Microsoft 365 or Office 365 address. Similarly, if any user who has not yet been migrated receives a message at their Microsoft 365 or Office 365 address, the message will get routed to their Google Workspace mailbox.

## Start a Google Workspace migration batch with the Classic Exchange admin center (Classic EAC)

1. In the Exchange Admin center, click **recipients**, and then click **migration**.

2. Click "New"  ![New](../media/ITPro_EAC_AddIcon.gif) to create a new migration batch, and then click **Migrate to Exchange Online**.

3. In the New Migration Batch window, select **G Suite (Gmail) migration**, and then click **Next**.

   ![Choose Gsuite](../media/gsuite-mig-13-eac-choose-gsuite.png)

4. Create a CSV file containing the set of all of the users you want to migrate. You will need its filename below. The allowed headers are:

   - EmailAddress (required). Contains the primary email address for an existing Microsoft 365 or Office 365 mailbox.

   - Username (optional). Contains the Gmail primary email address, if it differs from EmailAddress.

   ```CSV
   EmailAddress
   will@fabrikaminc.net
   user123@fabrikaminc.net
   ```

5. Under **Select the users**, click **Choose File** and navigate to the CSV file of all the users you are migrating in this batch. If your CSV file contains more columns besides the two mentioned above, click to select **Allow unknown columns in the CSV file**.

   ![Csv file](../media/gsuite-mig-14-eac-csv.png)

6. After selecting the CSV file, click **Open**. Back on the **new migration batch** page, click **Next**.

7. Enter the email address for the super admin within the Google Workspace environment. This is not the service account you just created, it should be the email address of the Google Workspace admin. This email address will be used to test connectivity between Google Workspace and Microsoft 365 or Office 365.

8. Under **Specify the service account credentials using the JSON key file**, click **Choose File**, and then select the JSON file that was downloaded automatically when you created your service account. This file contains the private key for the service account. Click **Open** to select the file, and then, back on the **new migration batch** page, click **Next**.

   ![user email address](../media/gsuite-mig-15-eac-user.png)

   > [!NOTE]
   > Click to select **Skip verification** if you don't want to verify the migration endpoint.

9. In the fields under **Move configuration**, name your migration batch, and enter the target delivery domain, which is the domain [you created](#create-a-subdomain-for-mail-routing-to-microsoft-365-or-office-365) for routing mail to the Microsoft 365 or Office 365 target organization from the Google Workspace source organization. Optionally, you can also specify any folders that should be excluded from the migration. When done, click **Next**.

   ![batch name](../media/gsuite-mig-16-eac-batch.png)

   > [!NOTE]
   > The target delivery domain you will want to use will not automatically show up in the dropdown - instead you should click within the text box and type it in. The target delivery domain must be different from the primary domain of the users in Google Workspace.

10. Under **Start the batch**, fill in the names or aliases of anyone who should be notified about the batch progress. Then select how you want to begin and complete the batch. When done, click **new**.

    ![start the batch](../media/gsuite-mig-17-eac-start.png)

11. After the batch status changes from **Syncing** to **Synced**, you need to complete the batch. The batch status will then be **Completed**.

    ![batch syncing](../media/gsuite-mig-18-eac-syncing.png)

    > [!NOTE]
    > When the batch starts, all the users to be migrated will be converted from MailUsers to Mailboxes. The Microsoft 365 or Office 365 Exchange license must be assigned only after this moment. You have 30 days to assign the license.

During completion, another incremental sync is run to copy any changes that have been made to the Google Workspace mailbox. Additionally, during completion, the forwarding address that routes mail from Microsoft 365 or Office 365 to Google Workspace is removed, and a forwarding address that routes mail from Google Workspace to Microsoft 365 or Office 365 is added. This ensures that any messages received by migrated users at their Google Workspace mailboxes will be sent to their new Microsoft 365 or Office 365 address. Similarly, if any user who has not yet been migrated receives a message at their Microsoft 365 or Office 365 address, the message will get routed to their Google Workspace mailbox.

## Start a Google Workspace migration with Exchange Online PowerShell

### Create a migration endpoint in Microsoft 365 or Office 365

1. [Connect to Exchange Online PowerShell](/powershell/exchange/connect-to-exchange-online-powershell).

2. Find the email address for the super admin within the Google Workspace environment. This email address will be used to test connectivity between Google Workspace and Microsoft 365 or Office 365. The following steps use 'admin123' as an example.

3. Run the following command:

   ```PowerShell
   Test-MigrationServerAvailability -Gmail -ServiceAccountKeyFileData $([System.IO.File]::ReadAllBytes("C:\\somepath\\yourkeyfile.json")) -EmailAddress admin123@fabrikaminc.net
   ```

4. Verify the test is successful.

5. If successful, run the following command:

   ```PowerShell
   New-MigrationEndpoint -Gmail -ServiceAccountKeyFileData $([System.IO.File]::ReadAllBytes("C:\\somepath\\yourkeyfile.json")) -EmailAddress admin123@fabrikaminc.net -Name gmailEndpoint
   ```

### Create a migration batch in Microsoft 365 or Office 365

1. [Connect to Exchange Online PowerShell](/powershell/exchange/connect-to-exchange-online-powershell).

2. Create a CSV file containing the set of all of the users you want to migrate. You will need its filename below. The allowed headers are:

   - EmailAddress (required). Contains the primary email address for an existing Microsoft 365 or Office 365 mailbox.

   - Username (optional). Contains the Gmail primary email address, if it differs from EmailAddress.

   ```CSV
   EmailAddress
   will@fabrikaminc.net
   user123@fabrikaminc.net
   ```

3. Run the following command:

   ```PowerShell
   New-MigrationBatch -SourceEndpoint gmailEndpoint -Name gmailBatch -CSVData $([System.IO.File]::ReadAllBytes("C:\\somepath\\gmail.csv")) -TargetDeliveryDomain "o365.fabrikaminc.net"
   ```

   > [!TIP]
   > See [New-MigrationBatch](/powershell/module/exchange/new-migrationbatch) for an explanation of all of the individual parameters you can use with this cmdlet.

4. Run the following command to start the migration batch:

   ```PowerShell
   Start-MigrationBatch -Identity gmailBatch
   ```

   > [!NOTE]
   > When the batch starts, all the users to be migrated will be converted from MailUsers to Mailboxes. The Microsoft 365 or Office 365 Exchange license must be assigned only after this moment. You have 30 days to assign the license.

### Complete the migration batch in Microsoft 365 or Office 365

When the migration batch has reached the state of **Synced**, it needs to be completed by running the `Complete-MigrationBatch` cmdlet.

During completion, another incremental sync is run to copy any changes that have been made to the Google Workspace mailbox. Additionally, the forwarding address that routes mail from Office 365 to Google Workspace is removed, and a forwarding address that routes mail from Google Workspace to Office 365 is added.

> [!NOTE]
> Forwarding addresses are not needed when doing a cutover migration from Google Workspace to Exchange Online.

## Finalizing your migration

After you have successfully migrated all of your Google Workspace users to Microsoft 365 or Office 365, you can switch your primary MX record to point to Microsoft 365 or Office 365. The update to the MX record will propagate slowly, taking up to the length of time in the record's previous TTL (time to live). At this point, you are free to decommission your source Google Workspace tenant.
