---
title: "Perform a G Suite migration"
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
audience: Admin
ms.topic: conceptual
ms.service: exchange-online
localization_priority: Normal
f1.keywords:
- NOCSH
description: "Summary: Instructions for performing a G Suite migration to Office 365."
---

# Perform a G Suite migration

You can migrate batches of users from G Suite to Office 365, allowing a migration project to be done in stages. This migration requires that you have Azure Active Directory (AD) Connect (formerly known as Office 365 Directory Synchronization, or DirSync) set up, or that you manually provision all of the MailUsers outside of the migration process. You must specify a list of users to migrate for each batch.

If you don't have Azure AD Connect in your environment, see [Deploy Office 365 Directory Synchronization in Microsoft Azure](https://docs.microsoft.com/office365/enterprise/deploy-office-365-directory-synchronization-dirsync-in-microsoft-azure) for an overview, and [Set up directory synchronization for Office 365](https://docs.microsoft.com/office365/enterprise/set-up-directory-synchronization) for set up instructions.

To manually provision mail-enabled users without DirSync, see [Manage mail users](../recipients-in-exchange-online/manage-mail-users.md) for more information.

All of the procedures in this article assume that your Office 365 domain has already been verified and your TXT records have been set up. For more information see [Set up your domain (host-specific instructions)](https://docs.microsoft.com/office365/admin/get-help-with-domains/set-up-your-domain-host-specific-instructions).

   > [!NOTE]
   > G Suite migration is not currently available for Office 365 US Government GCC High or DoD. 

## Overview of the process

Before beginning your migration, please review the following diagrams to understand how a G Suite staged migration works. The diagrams show how a fictitious company named Fabrikam, Inc., with the domain name *fabrikaminc.net* performed their migration.

![Original setup before G Suite migration](../media/gsuite-mig-original-setup.png)

Prior to their migration, the MX record for the base "fabrikaminc.net" domain points to the G Suite tenant or mail server where all or most of Fabrikam, Inc.'s users are. Note that users have their primary email addresses at that domain.

![Before the G Suite migration begins](../media/gsuite-mig-before-migration.png)

The MX record for the primary domain "fabrikaminc.net" still points to G Suite, where all the primary mailboxes reside. To prepare for the migration, new routing domains have been created: the *gsuite.fabrikaminc.net* domain points to G Suite and the *o365.fabrikaminc.net* domain points to Office 365.

On the G Suite side, aliases have been added for all of the users in the G Suite routing domain.  On the Office 365 side, MailUsers have been provisioned for all of the users from the G Suite tenant. The ExternalEmailAddress field for MailUsers on the Office 365 side were configured to point back to the primary mailbox using the address at the routing domain for the G Suite side.  Additionally, there should be aliases for the user in the O365 routing domain.

The green arrow indicates how, at this point in the migration, User 2 still contacts User 1 through their G Suite email addresses.

![During a single batch of a G Suite migration](../media/gsuite-mig-during-batch.png)

User 1 and User 2 are part of the first migration batch to Office 365, while User 3 and User 4 will be part of a later batch. The MX record for the primary domain "fabrikaminc.net" still points to G Suite, where all the primary mailboxes still reside. Because User 1 and User 2 have had their migrations started, they've been converted from MailUsers to Mailboxes on the Office 365 side.

The ExternalEmailAddress for each user has been moved to a ForwardingSmtpAddress, so that messages sent to User 1 and User 2 will be delivered back to their source mailboxes on the G Suite side by rerouting the message back to the G Suite routing domain. This is indicated by the red arrows in the above diagram. Mail is still being synced from the source G Suite side to the Office 365 side.

![After a single batch of a G Suite migration](../media/gsuite-mig-after-batch.png)

The MX record for the primary domain "fabrikaminc.net" still points to G Suite. Now that User 1 and User 2 have been fully migrated to Office 365, they should start working out of Office 365. On the G Suite side, automatic mail forwarding has been set up for migrated users, so that new emails sent to their G Suite address will be delivered instead to the Office 365 address via the routing domain. This is shown by the green arrows in the above diagram.

Meanwhile, the forwarding address has been removed from the Office 365 user object, so emails will be delivered to that user in the Office 365 routing domain (as shown by the red arrows above).

![After G Suite migration is complete](../media/gsuite-mig-after-migration.png)

After all migration batches have been completed, all users can use their migrated mailboxes on Office 365 as their primary mailbox. A manual MX record update for the primary domain "fabrikaminc.net" then points to the Office 365 tenant instead of the G Suite tenant.  The routing domains and extra aliases can now be removed, as can the G Suite tenant. The migration of mail, calendar, and contacts from G Suite to Office 365 is now complete.

## Migration limitations

> [!NOTE]
> The largest single email message that can be migrated is based on the transport configuration for your configuration. The default limit is 35 MB. To increase this limit, see [Office 365 now supports larger email messages](https://www.microsoft.com/microsoft-365/blog/2015/04/15/office-365-now-supports-larger-email-messages-up-to-150-mb/).

Throughput limitations for contacts and calendars completely depend on the quota restrictions for your tenant's service account on the Google G Suite side.

Additional migration limitations are described in the following table:


|Data type  |Limitations  |
|---------|---------|
|Mail     |Vacation settings, Automatic reply settings, Filters/Rules will not be migrated         |
|Meeting rooms     |Room bookings will not be migrated         |
|Calendar     |Shared calendars, cloud attachments, Google Hangout links, and event colors will not be migrated         |
|Contacts     |A maximum of three email addresses per contact are migrated over         |
|Contacts     |Gmail tags, contact URLs, and custom tags will not be migrated         |

> [!TIP]
> If you will be [starting your migration batch with Exchange Online Powershell](#start-a-g-suite-migration-with-exchange-online-powershell), as described later in this article, you can use the `-ExcludeFolder` parameter to prevent certain folders from being migrated. This will reduce the amount of data in your migration, as well as the size of a user's new Exchange Online mailbox. You can identify folders you don't want to migrate by name, and you can also identify Gmail labels that apply to multiple messages in order to exclude those messages from the migration. For more information on using `-ExcludeFolder`, see [New-MigrationBatch](https://docs.microsoft.com/powershell/module/exchange/move-and-migration/new-migrationbatch?view=exchange-ps).  

## Create a Google Service Account

> [!IMPORTANT]
> Use Chrome to create your Google Service account. Other browsers may not allow you to do this properly. <br/><br/> Because elements of the G Suite user interface can change over time, the screens you see might vary from the examples in this section. The locations of certain fields may vary as well. Please look at Google's Documentation for how to [Create a Service Account](https://support.google.com/a/answer/7378726) for clarifications in case the UI has changed significantly.

1. In Chrome, go to the [Developer page for Service Accounts](https://console.developers.google.com/iam-admin/serviceaccounts) and sign in as a Google user (such as the G Suite admin).

2. Click **Create** to create and name a new project for the onboarding (such as "Gsuite migration project"), or click **Select** to select an existing project.

   ![Create a project](../media/gsuite-mig-1-newproject.png)

3. Click **Create Service Account** and, in **Service account name**, give the service account a name, such as "Gmail Onboarding." Click **Create**.

   ![Create service account](../media/gsuite-mig-2-newservice.png)

4. On the **Service account permissions (optional)** screen, click **Continue**.

5. Under **Create key (optional)** click **Create Key**.

   ![Create key](../media/gsuite-mig-3-createkey.png)

6. Under **Key type**, make sure **JSON** is selected, and then click **Create**.

   ![JSON key](../media/gsuite-mig-4-json.png)

7. Keep track of the JSON keyfile that is automatically downloaded, as you will need its filename during the steps under [Create a migration endpoint in Office 365](#create-a-migration-endpoint-in-office-365). Click **Done**.

8. Click on the **Email** for the Service Account you just created to enter the details page. Alternately, you can click the dots under the **Actions** column and click on the **Edit** action. On the Service account details page, note the **Unique ID**. This is the ClientId that you will provide later in the instructions for [Grant access to the service account for your Google tenant](#grant-access-to-the-service-account-for-your-google-tenant).

   ![Unique ID](../media/gsuite-mig-6-unique-id.png)

9. If you see an area that says **Show Domain-Wide Delegation**, click to expand that section. Then, enable the checkbox for **Enable G Suite Domain-wide Delegation**, and then click **Save**. Google is currently in the process of updating this UI, so you may not see such an option. If you don't see such an option, then this is enabled by default.

   ![Domain-Wide Delegation](../media/gsuite-mig-7-enable-domain-delegation.png)

## Enable API usage in your project

If your project doesn't already have all of the required APIs enabled, you must enable them.

1. Go to the [Developer page for API Library](https://console.developers.google.com/apis/library) and sign in as the Google user you used above in **Create a Google Service Account**.

2. Select the project that you used above.

3. Search for the following APIs, and then for each one, if necessary, click **Enable** to enable them for your project:

   - Gmail API

   - Google Calendar API

   - Contacts API

## Grant access to the service account for your Google tenant

1. Go to the [G Suite Admin page](https://admin.google.com/AdminHome) and sign in as G Suite admin for your tenant.

2. Click **Security**, then click **Advanced settings**, and then click **Manage API client access**.

3. In **Client Name**, type the ClientId for the service account you created in the [Create a Google Service Account](#create-a-google-service-account) section above.

4. In **API Scopes**, add the required scopes in comma-separated format, with no spaces in between. For example, `https://mail.google.com/,https://www.googleapis.com/auth/calendar,https://www.google.com/m8/feeds/,https://www.googleapis.com/auth/gmail.settings.sharing`. If the API Scopes are entered incorrectly, the resulting list won't match and the migration process will fail later, after you start the migration batch.

5. Click **Authorize**. Verify that the resulting list shows "Email (Read/Write/Send)", "Calendar (Read-Write)", "Contacts (Read/Write)", and "<https://www.googleapis.com/auth/gmail.settings.sharing>".

   ![Grant service account access](../media/gsuite-mig-8-grant-service-account-access.png)

   > [!NOTE]
   > It may take a substantial length of time for these settings to propagate (anywhere from 15 minutes to 24 hours).

## Create a sub-domain for mail routing to Office 365

1. Go to the [G Suite Admin page](https://admin.google.com/AdminHome) and sign in as a G Suite admin for your tenant.

2. Click **Domains**, and then **Add/remove domains**, and then click **Add a domain or a domain alias**.

3. Select **Add another domain**. Enter the domain that you will use for routing mails to Office 365. A sub-domain of your primary domain is recommended (such as "o365.fabrikaminc.net" when "fabrikaminc.net" is your primary domain) so that it will be automatically verified. Keep track of the name of the domain you enter because you will need it for the following steps, and later in the instructions as the Target Delivery Domain when you [Create a migration batch in Office 365](#create-a-migration-batch-in-office-365).

   ![Add another domain](../media/gsuite-mig-11-sub-domain-gsuite.png)

4. For your newly created domain, make sure that the status is **Verified**. Follow any steps required to get the domain to a verified state. Note that if you chose a subdomain of your primary domain in step 3 above, your new domain should have been verified automatically.

   ![Skip Google mx](../media/gsuite-mig-10-skip-google-mx.png)

5. Click **Skip Google MX setup**, and then click **I use another mail server**. This other mail server will be Office 365.

6. Log into your DNS provider and update your DNS records so that you have an MX record at the domain you created above in step 3, pointing to Office 365. Ensure that this domain that you created above is an accepted domain in Office 365. Follow the instructions in [Add a domain to Office 365](https://docs.microsoft.com/office365/admin/setup/add-domain?view=o365-worldwide) to add the Office 365 routing domain ("o365.fabrikaminc.net") to your organization and to configure DNS to route mail to Office 365.

## Create a sub-domain for mail routing to your G Suite domain

1. Go to the [G Suite Admin page](https://admin.google.com/AdminHome) and sign in as a G Suite admin for your tenant.

2. Click **Domains**, and then **Add/remove domains**, and then click **Add a domain or a domain alias**.

3. Select **Add a domain alias of...** your domain. Enter the domain that you will use for routing mails to G Suite. A sub-domain of your primary domain is recommended (such as "gsuite.fabrikaminc.net" when "fabrikaminc.net" is your primary domain) so that it will be automatically verified.

   ![Add another domain](../media/gsuite-mig-9-sub-domain-0365.png)

4. For your newly created domain, make sure that the status is **Verified**. Follow any steps required to get the domain to a verified state. Note that if you chose a subdomain of your primary domain in step 3 above, your new domain should have been verified automatically.

   ![Set up Google mx](../media/gsuite-mig-12-set-up-google-mx.png)

5. Click **Set up Google MX records**, and then follow the instructions that are listed for your DNS provider.

   > [!NOTE]
   > It may take up to 24 hours for Google to propagate this setting to all of the users in your organization.

   > [!IMPORTANT]
   > If you are using non-default Transport settings in your Office 365 organization, you should check that mail flow will work from Office 365 to G Suite. Be sure that either your default Remote Domain ("\*") has Automatic Forwarding enabled, or that there is a new Remote Domain for your G Suite routing domain (e.g. "gsuite.fabrikaminc.net") that has Automatic Forwarding enabled.

## Provision users in O365

Once your G Suite environment has been properly configured, you can complete your migration in the Exchange admin center or through the Exchange Online PowerShell.

Before proceeding with either method, make sure that MailUsers have been provisioned for every user in the organization who will be migrated (either now or eventually). If any users aren't provisioned, provision them using the instructions in [Manage mail users](https://docs.microsoft.com/exchange/recipients-in-exchange-online/manage-mail-users).

We recommend that the primary address (sometimes referred to as the "User Id") for each user be at the primary domain (such as "will@fabrikaminc.net"). Typically, this means that the primary email address should match between O365 and G Suite. If any user is provisioned with a different domain for their primary address, then that user should at least have a proxy address at the primary domain. Each user should have their `ExternalEmailAddress` point to the user in their G Suite routing domain ("will@gsuite.fabrikaminc.net"). The users should also have a proxy address that will be used for routing to their Office 365 routing domain (such as "will@o365.fabrikaminc.net").

## Start a G Suite migration batch with the Exchange admin center (EAC)

1. In the Exchange Admin center, click **recipients**, and then click **migration**.

2. Click "New"  ![New](../media/ITPro_EAC_AddIcon.gif) to create a new migration batch, and then click **Migrate to Exchange Online**.

3. In the New Migration Batch window, select **G Suite (Gmail) migration**, and then click **Next**.

   ![Choose Gsuite](../media/gsuite-mig-13-eac-choose-gsuite.png)

4. Create a CSV file containing the set of all of the users you want to migrate. You will need its filename below. The allowed headers are:

   - EmailAddress (required). Contains the primary email address for an existing Office 365 mailbox.

   - Username (optional). Contains the Gmail primary email address, if it differs from EmailAddress.

   ```CSV
   EmailAddress
   will@fabrikaminc.net
   user123@fabrikaminc.net
   ```

5. Under **Select the users**, click **Choose File** and navigate to the CSV file of all the users you are migrating in this batch. If your CSV file contains additional columns besides the two mentioned above, click to select **Allow unknown columns in the CSV file**.

   ![Csv file](../media/gsuite-mig-14-eac-csv.png)

6. After selecting the CSV file, click **Open**. Back on the **new migration batch** page, click **Next**.

7. Enter the email address for the super admin within the G Suite environment. This email address will be used to test connectivity between G Suite and Office 365.

8. Under **Specify the service account credentials using the JSON key file**,click **Choose File**, and then select the JSON file that was downloaded automatically when you created your service account. This file contains the private key for the service account. Click **Open** to select the file, and then, back on the **new migration batch** page, click **Next**.

   ![user email address](../media/gsuite-mig-15-eac-user.png)

   > [!NOTE]
   > Click to select **Skip verification** if you don't want to verify the migration endpoint.

9. In the fields under **Move configuration**, name your migration batch and enter the target delivery domain, which is the domain [you created](#create-a-sub-domain-for-mail-routing-to-office-365) for routing mail to the Office 365 target organization from the G Suite source organization. Optionally, you can also specify a bad item limit and a large item limit, and you can specify any folders that should be excluded from the migration. When done, click **Next**.

   ![batch name](../media/gsuite-mig-16-eac-batch.png)

   > [!NOTE]
   > The target delivery domain you will want to use will not automatically show up in the dropdown - instead you should click within the text box and type it in.

10. Under **Start the batch**, fill in the names or aliases of anyone who should be notified about the batch progress. Then select how you want to begin and complete the batch. When done, click **new**.

    ![start the batch](../media/gsuite-mig-17-eac-start.png)

11. After the batch status changes from **Syncing** to **Synced**, you can complete the batch. The batch status will then be **Completed**.

    ![batch syncing](../media/gsuite-mig-18-eac-syncing.png)

During completion, another incremental sync is run to copy any changes that have been made to the G Suite mailbox. Additionally, the forwarding address that routes mail from O365 to G Suite is removed, and a forwarding address that routes mail from G Suite to O365 is added. This ensures that any  messages received by migrated users at their G Suite mailboxes will be sent to their new Office 365 address. Similarly, if any user who has not yet been migrated receives a message at their Office 365 address, the message will get routed to their G Suite mailbox.

## Start a G Suite migration with Exchange Online Powershell

### Create a migration endpoint in Office 365

1. Connect to the service using Remote Powershell. See [Connect to Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/exchange-online/connect-to-exchange-online-powershell/connect-to-exchange-online-powershell?view=exchange-ps) for more information.

2. Find the email address for the super admin within the G Suite environment. This email address will be used to test connectivity between G Suite and Office 365. The following steps use 'admin123' as an example.

3. Run the following command:

   ```PowerShell
   Test-MigrationServerAvailability -Gmail -ServiceAccountKeyFileData $([System.IO.File]::ReadAllBytes("C:\\somepath\\yourkeyfile.json")) -EmailAddress admin123@fabrikaminc.net
   ```

4. Verify the test is successful.

5. If successful, run the following command:

   ```PowerShell
   New-MigrationEndpoint -Gmail -ServiceAccountKeyFileData $([System.IO.File]::ReadAllBytes("C:\\somepath\\yourkeyfile.json")) -EmailAddress admin123@fabrikaminc.net -Name gmailEndpoint
   ```

### Create a migration batch in Office 365

1. Connect to the service using Remote Powershell.

2. Create a CSV file containing the set of all of the users you want to migrate. You will need its filename below. The allowed headers are:

   - EmailAddress (required). Contains the primary email address for an existing Office 365 mailbox.

   - Username (optional). Contains the Gmail primary email address, if it differs from EmailAddress.

   ```CSV
   EmailAddress
   will@fabrikaminc.net
   user123@fabrikaminc.net
   ```

3. Run the following command:

   ```CSV
   New-MigrationBatch -SourceEndpoint gmailEndpoint -Name gmailBatch -CSVData $([System.IO.File]::ReadAllBytes("C:\\somepath\\gmail.csv")) -TargetDeliveryDomain "o365.fabrikaminc.net"
   ```

   > [!TIP]
   > See [New-MigrationBatch](https://docs.microsoft.com/powershell/module/exchange/move-and-migration/new-migrationbatch?view=exchange-ps) for an explanation of all of the individual parameters you can use with this cmdlet.

4. Start the migration batch.

### Complete the migration batch in Office 365

When the migration batch has reached the state of **Synced**, it can be completed by running the `Complete-MigrationBatch` cmdlet.

During completion, another incremental sync is run to copy any changes that have been made to the G Suite mailbox.  Additionally, the forwarding address that routes mail from O365 to G Suite is removed, and a forwarding address that routes mail from G Suite to O365 is added.

> [!NOTE]
> Forwarding addresses are not needed when doing a cutover migration from G Suite to Exchange Online.

## Finalizing your migration

After you have successfully migrated all of your G Suite users to Office 365, you can switch your primary MX record to point to Office 365. The update to the MX record will propagate slowly, taking up to the length of time in the record's previous TTL (time to live).  At this point, you are free to decommission your source G Suite tenant.
