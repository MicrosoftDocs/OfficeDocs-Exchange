---
localization_priority: Normal
ms.topic: conceptual
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 9496e93c-1e59-41a8-9bb3-6e8df0cd81b4
ms.reviewer: 
description: "As part of a Microsoft 365 or Office 365 deployment, you can migrate the contents of user mailboxes from a source email system to Microsoft 365 or Office 365. When you do this all at one time, it's called a cutover migration. Choosing a cutover migration is suggested when:"
title: Migrate email using the Exchange cutover method
ms.collection: 
- exchange-online
- M365-email-calendar
search.appverid:
- MET150
- MOE150
- MED150
- MBS150
- BCS160
audience: Admin
f1.keywords:
- CSH
ms.custom: Adm_O365
ms.service: exchange-online
manager: serdars

---

# Migrate email using the Exchange cutover method

As part of a Microsoft 365 or Office 365 deployment, you can migrate the contents of user mailboxes from a source email system to Microsoft 365 or Office 365. When you do this all at one time, it's called a cutover migration. Choosing a cutover migration is suggested when:

- Your current on-premises Exchange organization is Microsoft Exchange Server 2003 or later.

- Your on-premises Exchange organization has fewer than 2,000 mailboxes.

    > [!NOTE]
    > Even though cutover migration supports moving up to 2000 mailboxes, due to length of time it takes to create and migrate 2000 users, it is more reasonable to migrate 150 users or fewer.

## Plan for migration

Setting up an email cutover migration to Microsoft 365 or Office 365 requires careful planning. Before you begin, here are a few things to consider:

- You can move your entire email organization to Microsoft 365 or Office 365 over a few days and manage user accounts in Microsoft 365 or Office 365.

- A maximum of 2,000 mailboxes can be migrated to Microsoft 365 or Office 365 by using a cutover Exchange migration. However, it is recommended that you only migrate 150 mailboxes.

- The primary domain name used for your on-premises Exchange organization must be an accepted as a domain owned by you in your Microsoft 365 or Office 365 organization.

- After the migration is complete, each user who has an on-premises Exchange mailbox also will be a new user in Microsoft 365 or Office 365, but you must still assign licenses to users whose mailboxes are migrated.

    > [!NOTE]
    > When migrating from Exchange 2003, TCP port 6001, 6002 and 6004 need to be open on the Exchange 2003 side.

## Impact to users

After your on-premises and Microsoft 365 or Office 365 organizations are set up for a cutover migration, post-setup tasks could impact your users.

- **Administrators or users must configure desktop computers**: Make sure that desktop computers are updated and set up for use with Microsoft 365 or Office 365. These actions allow users to use local user credentials to sign in to Microsoft 365 or Office 365 from desktop applications. Users with permission to install applications can update and set up their own desktops. Or updates can be installed for them. After updates are made, users can send email from Outlook 2013, Outlook 2010, or Outlook 2007.

- **Potential delay in email routing**: Email sent to on-premises users whose mailboxes were migrated to Microsoft 365 or Office 365 are routed to their on-premises Exchange mailboxes until the MX record is changed.

## How does cutover migration work?

The main steps you perform for a cutover migration are shown in the following illustration.

![Process for performing a cutover email migration to Microsoft 365 or Office 365](media/a607954b-1ab6-40e6-becc-d61ad5a35d69.png)

1. The administrator communicates upcoming changes to users and verifies domain ownership with the domain registrar.

2. The administrator prepares the servers for a cutover migration and creates empty mail-enabled security groups in Microsoft 365 or Office 365.

3. The administrator connects Microsoft 365 or Office 365 to the on-premises email system (this is called creating a migration endpoint).

4. The administrator migrates the mailboxes and then verifies the migration.

5. Grant Microsoft 365 or Office 365 licenses to your users.

6. The administrator configures the domain to begin routing email directly to Microsoft 365 or Office 365.

7. The administrator verifies that routing has changed, and then deletes the cutover migration batch.

8. The administrator completes post-migration tasks in Microsoft 365 or Office 365 (assigns licenses to users and creates an Autodiscover Domain Name System (DNS) record), and optionally decommissions the on-premises Exchange servers.

9. The administrator sends a welcome letter to users to tell them about Microsoft 365 or Office 365 and to describe how to sign in to their new mailboxes.

## Ready to run a cutover migration?

Expand the sections below and follow the steps.

## Prepare for a cutover migration

Before you migrate mailboxes to Microsoft 365 or Office 365 by using a cutover migration, there are a few changes to your Exchange Server environment you must complete first.

> [!NOTE]
> If you have turned on directory synchronization, you need to turn it off before you can perform a cutover migration. You can do this by using PowerShell. For instructions, see [Turn off directory synchronization](https://docs.microsoft.com/office365/enterprise/turn-off-directory-synchronization).

1. **Configure Outlook Anywhere on your on-premises Exchange Server**: The email migration service uses Outlook Anywhere (also known as RPC over HTTP), to connect to your on-premises Exchange Server. Outlook Anywhere is automatically configured for Exchange 2013. For information about how to set up Outlook Anywhere for Exchange 2010, Exchange 2007, and Exchange 2003, see the following:

   - [Exchange 2010: Enable Outlook Anywhere](https://docs.microsoft.com/previous-versions/office/exchange-server-2010/bb123542(v=exchg.141))

   - [Exchange 2007: How to Enable Outlook Anywhere](https://docs.microsoft.com/previous-versions/office/exchange-server-2007/bb123889(v=exchg.80))

   - [How to configure Outlook Anywhere with Exchange 2003](https://docs.microsoft.com/previous-versions/office/exchange-server-2007/aa996922(v=exchg.80))

2. You must use a certificate issued by a trusted certification authority (CA) with your Outlook Anywhere configuration in order for Microsoft 365 or Office 365 to run a cutover migration. For cutover migration you will to add the Outlook Anywhere and Autodiscover services to your certificate. For instructions on how to set up certificates, see:

   - [Add an SSL certificate to Exchange 2013](add-an-ssl-certificate-to-exchange-2013.md)

   - [Add an SSL certificate to Exchange 2010](add-an-ssl-certificate-to-exchange-2010.md)

   - [Add an SSL certificate to Exchange 2007](add-an-ssl-certificate-to-exchange-2007.md)

3. **Optional: Verify that you can connect to your Exchange organization using Outlook Anywhere**: Try one of the following methods to test your connection settings.

   - Use Outlook from outside your corporate network to connect to your on-premises Exchange mailbox.

   - Use the [Microsoft Exchange Remote Connectivity Analyzer](https://docs.microsoft.com/connectivity-analyzer/exchange-remote-connectivity-analyzer-tool) to test your connection settings. Use the Outlook Anywhere (RPC over HTTP) or Outlook Autodiscover tests.

   - Wait for the connection to automatically be tested when you connect Microsoft 365 or Office 365 to your email system later in this procedure.

4. **Set permissions**: The on-premises user account that you use to connect to your on-premises Exchange organization (also called the migration administrator) must have the necessary permissions to access the on-premises mailboxes that you want to migrate to Microsoft 365 or Office 365. This user account is used when you connect Microsoft 365 or Office 365 to your email system later in this procedure.

5. To migrate the mailboxes, the admin must have one of the following permissions:

   - The migration administrator must be assigned the **FullAccess** permission for each on-premises mailbox.

   or

   - The migration administrator must be assigned the **Receive As** permission on the on-premises mailbox database that stores user mailboxes.

    For instructions about how to set these permissions, see [Assign Exchange permissions to migrate mailboxes to Microsoft 365 or Office 365](assign-permissions-for-migration.md).

6. **Disable Unified Messaging (UM)**: If UM is turned on for the on-premises mailboxes you're migrating, turn off UM before migration. [Turn on Cloud Voicemail](https://docs.microsoft.com/microsoftteams/set-up-phone-system-voicemail) for your users after the migration is complete.

7. **Create security groups and clean up delegates**: Because the email migration service can't detect whether on-premises Active Directory groups are security groups, it can't provision any migrated groups as security groups in Microsoft 365 or Office 365. If you want to have security groups in Microsoft 365 or Office 365, you must first provision an empty mail-enabled security group in Microsoft 365 or Office 365 before starting the cutover migration.

   Additionally, this migration method only moves mailboxes, mail users, mail contacts, and mail-enabled groups. If any other Active Directory object, such as user mailbox that is not migrated to Microsoft 365 or Office 365 is assigned as a manager or delegate to an object being migrated, you must remove them from the object before migration.

## Step 1: Verify you own the domain

During the migration, the Simple Mail Transfer Protocol (SMTP) address of each on-premises mailbox is used to create the email address for a new Microsoft 365 or Office 365 mailbox. To run a cutover migration, the on-premises domain must be a verified domain in your Microsoft 365 or Office 365 organization.

1. Sign in to Microsoft 365 or Office 365 with your work or school account.

2. Choose **Setup** \> **Domains**.

3. On the **Domains-** page, click **Add domain** to start the domain wizard.

    ![Choose Add domain](media/b0267b62-3f20-4c76-be75-40f9c2274433.png)

4. On the **Add a domain** page, type in the domain name (for example, Contoso.com) you use for your on-premises Exchange organization, and then choose **Next**.

5. On the **Verify domain** page, select either **Sign in to GoDaddy** (if your DNS records are managed by GoDaddy) or **Add a TXT record instead** for any other registrars \> **Next**.

6. Follow the instructions provided for your DNS hosting provider. The TXT record usually is chosen to verify ownership.

   You can also find the instructions in [Add DNS records to connect your domain](https://docs.microsoft.com/microsoft-365/admin/get-help-with-domains/create-dns-records-at-any-dns-hosting-provider).

   After you add your TXT or MX record, wait about 15 minutes before proceeding to the next step.

7. In the Office 365 domain wizard, choose **done, verify now**, and you'll see a verification page. Choose **Finish**.

   If the verification fails at first, wait awhile, and try again.

   Do not continue to the next step in the domain wizard. You now have verified that you own the on-premises Exchange organization domain and are ready to continue with an email migration.

## Step 2: Connect Microsoft 365 or Office 365 to your email system

A migration endpoint contains the settings and credentials needed to connect the on-premises server that hosts the mailboxes you're migrating with Microsoft 365 or Office 365. The migration endpoint also defines the number of mailboxes to migrate simultaneously. For a cutover migration, you'll create an Outlook Anywhere migration endpoint.

1. Go to the Exchange admin center.

2. In the Exchange admin center, go to **Recipients** \> **Migration**.

3. Choose **More** ![More icon](media/148718eb-ebbd-4aa5-99bb-bcf5a6d7d942.gif) \> **Migration endpoints**.

   ![Select Migration endpoint.](media/474a2e9a-a7f1-4657-8a09-eeec45e106f5.png)

4. On the **Migration endpoints** page, choose **New** ![New icon](media/457cd93f-22c2-4571-9f83-1b129bcfb58e.gif).

5. On the **Select the migration endpoint type** page, choose **Outlook Anywhere** \> **Next**.

6. On the **Enter on-premises account credentials** page, enter information in the following boxes:

   - **Email address**: Type the *email address* of any user in the on-premises Exchange organization that will be migrated. Microsoft 365 or Office 365 will test the connectivity to this user's mailbox.

   - **Account with privileges**: Type the *username* (domain\username format or an email address) for an account that has the necessary administrative permissions in the on-premises organization. Microsoft 365 or Office 365 will use this account to detect the migration endpoint and to test the permissions assigned to this account by attempting to access the mailbox with the specified email address.

   - **Password of account with privileges**: Type the *password* for the account with privileges that is the administrator account.

7. Choose **Next** and do one of the following:

   - If Microsoft 365 or Office 365 successfully connects to the source server, the connection settings are displayed. Choose **Next**.

     ![Confirmed connection for Outlook Anywhere endpoint.](media/eea5e4c3-73b9-44be-abbe-e7387aea578a.JPG)

   - If the test connection to the source server isn't successful, provide the following information:

   - **Exchange server**: Type the *fully qualified domain name* (FQDN) for the on-premises Exchange Server. This is the host name for your Mailbox server. For example, EXCH-SRV-01.corp.contoso.com.

   - **RPC proxy server**: Type the *FQDN* for the RPC proxy server for Outlook Anywhere. Typically, the proxy server is the same as your Outlook on the web (formerly known as Outlook Web App) URL. For example, mail.contoso.com, which is also the URL for the proxy server that Outlook uses to connect to an Exchange Server

8. On the **Enter general information** page, type a *Migration endpoint name*, for example, Test5-endpoint. Leave the other two boxes blank to use the default values.

   ![Migration endpoint name.](media/990cd22d-748c-477d-b3f8-66f30b256475.jpg)

9. Choose **New** to create the migration endpoint.

   To validate your Exchange Online is connected to the on-premises server, you can run the command in Example 4 of [Test-MigrationServerAvailability](https://docs.microsoft.com/powershell/module/exchange/Test-MigrationServerAvailability).

## Step 3: Create the cutover migration batch

In a cutover migration, on-premises mailboxes are migrated to Microsoft 365 or Office 365 in a single migration batch.

1. In the Exchange admin center, go to **Recipients** \> **Migration**.

2. Choose **New** ![New icon](media/457cd93f-22c2-4571-9f83-1b129bcfb58e.gif) \> **Migrate to Exchange Online**.

   ![Select Migrate to Exchange Online](media/d5af665e-498d-4f18-8761-fc69897b389d.png)

3. On the **Select a migration type** page, choose **Cutover migration** \> **next**.

4. On the **Confirm the migration endpoint** page, the migration endpoint information is listed. Verify the information and then choose **next**.

   ![New migration batch with confirmed endpoint.](media/1c7ff17e-6754-4c98-8b29-5a479239df13.JPG)

5. On the **Move configuration** page, type the *name* (cannot contain spaces or special characters) of the migration batch, and then choose **next**. The batch name is displayed in the list of migration batches on the **Migration** page after you create the migration batch.

6. On the **Start the batch** page, choose one of the following:

   - **Automatically start the batch**: The migration batch is started as soon as you save the new migration batch with a status of **Syncing**.

   - **Manually start the batch later**: The migration batch is created but is not started. The status of the batch is set to **Created**. To start a migration batch, select it on the migration dashboard, and then choose **Start**.

7. Choose **new** to create the migration batch.

   The new migration batch is displayed on the migration dashboard.

## Step 4: Start the cutover migration batch

If you created a migration batch and configured it to be started manually, you can start it by using the Exchange admin center.

1. In the Exchange admin center, go to **Recipients** \> **Migration**.

2. On the migration dashboard, select the batch and then choose **Start**.

3. If a migration batch starts successfully, its status on the migration dashboard changes to **Syncing**.

   ![Migration batch is syncing](media/c6789813-6822-4a28-a47c-2c62e1da9b8c.png)

### Verify the synchronization worked

You'll be able to follow the sync status on the migration dashboard. If there are errors, you can view a log file that gives you more information about them.

You can also verify that the users get created in the Microsoft 365 admin center as the migration proceeds.

After the migration is done, the sync status is **Synced**.

## Optional: Reduce email delays

 Although this task is optional, doing it can help avoid delays in the receiving email in the new Microsoft 365 or Office 365 mailboxes.

When people outside of your organization send you email, their email systems don't double-check where to send that email every time. Instead, their systems save the location of your email system based on a setting in your DNS server known as a time-to-live (TTL). If you change the location of your email system before the TTL expires, the sender's email system tries to send email to the old location before figuring out that the location changed. This location change can result in a mail delivery delay. One way to avoid this is to lower the TTL that your DNS server gives to servers outside of your organization. This will make the other organizations refresh the location of your email system more often.

Most email systems ask for an update each hour if a short interval such as 3,600 seconds (one hour) is set. We recommend that you set the interval at least this low before you start the email migration. This setting allows all the systems that send you email enough time to process the change. Then, when you make the final switch over to Microsoft 365 or Office 365, you can change the TTL back to a longer interval.

The place to change the TTL setting is on your email system's MX record. This lives on your public-facing DNS system. If you have more than one MX record, you need to change the value on each record to 3,600 seconds or less.

If you need some help configuring your DNS settings, see [Add DNS records to connect your domain](https://docs.microsoft.com/microsoft-365/admin/get-help-with-domains/create-dns-records-at-any-dns-hosting-provider).

## Step 5: Route your email directly to Microsoft 365 or Office 365

Email systems use a DNS record called an MX record to figure out where to deliver emails. During the email migration process, your MX record was pointing to your source email system. Now that the email migration to Microsoft 365 or Office 365 is complete, it's time to point your MX record at Microsoft 365 or Office 365. This helps make sure that email is delivered to your Microsoft 365 or Office 365 mailboxes. Moving the MX record will also let you turn off your old email system when you're ready.

For many DNS providers, there are specific instructions to change your MX record. If your DNS provider isn't included, or if you want to get a sense of the general directions, [general MX record instructions](https://docs.microsoft.com/microsoft-365/admin/get-help-with-domains/create-dns-records-at-any-dns-hosting-provider) are provided as well.

It can take up to 72 hours for the email systems of your customers and partners to recognize the changed MX record. Wait at least 72 hours before you proceed to the next task: Delete the cutover migration batch.

## Step 6: Delete the cutover migration batch

After you change the MX record and verify that all email is being routed to Microsoft 365 or Office 365 mailboxes, notify the users that their mail is going to Microsoft 365 or Office 365. After this you can delete the cutover migration batch. Verify the following before you delete the migration batch.

- All users are using Microsoft 365 or Office 365 mailboxes. After the batch is deleted, mail sent to mailboxes on the on-premises Exchange Server isn't copied to the corresponding Microsoft 365 or Office 365 mailboxes.

- Microsoft 365 or Office 365 mailboxes were synchronized at least once after mail began being sent directly to them. To do this, make sure that the value in the **Last Synced Time** box for the migration batch is more recent than when mail started being routed directly to Microsoft 365 or Office 365 mailboxes.

When you delete a cutover migration batch, the migration service cleans up any records related to the migration batch and then deletes the migration batch. The batch is removed from the list of migration batches on the migration dashboard.

1. In the Exchange admin center, go to **Recipients** \> **Migration**.

2. On the migration dashboard, select the batch, and then choose **Delete**.

   > [!NOTE]
   > It can take a few minutes or the batch to be removed.

3. In the Exchange admin center, go to **Recipients** \> **Migration**.

4. Verify that the migration batch is no longer listed on the migration dashboard.

## Step 7: Assign licenses to Microsoft 365 and Office 365 users

**Activate user accounts for the migrated accounts by assigning licenses**: If you don't assign a license, the mailbox is disabled when the grace period ends (30 days). To assign a license in the Microsoft 365 admin center, see [Add users individually or in bulk](https://docs.microsoft.com/microsoft-365/admin/add-users/add-users).

## Complete post migration tasks

After migrating mailboxes to Microsoft 365 or Office 365, there are post-migration tasks that must be completed.

1. **Create an Autodiscover DNS record so users can easily get to their mailboxes**: After all on-premises mailboxes are migrated to Microsoft 365 or Office 365, you can configure an Autodiscover DNS record for your Microsoft 365 or Office 365 organization to enable users to easily connect to their new Microsoft 365 or Office 365 mailboxes with Outlook and mobile clients. This new Autodiscover DNS record has to use the same namespace that you're using for your Microsoft 365 or Office 365 organization. For example, if your cloud-based namespace is cloud.contoso.com, the Autodiscover DNS record you need to create is autodiscover.cloud.contoso.com.

   If you keep your Exchange Server, you should also make sure that Autodiscover DNS CNAME record has to point to Microsoft 365 or Office 365 in both internal and external DNS after the migration so that the Outlook client will to connect to the correct mailbox. Replace \<ServerName\> with the name of the Client Access server and run the following command in the Exchange Management Shell to prevent client connections to the server. You'll need to run the command on every Client Access server.

   ```PowerShell
   Set-ClientAccessServer -Identity <ServerName> -AutoDiscoverServiceInternalUri $null
   ```

   Microsoft 365 or Office 365 uses a CNAME record to implement the Autodiscover service for Outlook and mobile clients. The Autodiscover CNAME record must contain the following information:

   - **Alias:** autodiscover

   - **Target:** autodiscover.outlook.com

   For more information, see [Add DNS records to connect your domain](https://docs.microsoft.com/microsoft-365/admin/get-help-with-domains/create-dns-records-at-any-dns-hosting-provider).

2. **Decommission on-premises Exchange Servers**: After you've verified that all email is being routed directly to the Microsoft 365 or Office 365 mailboxes, and no longer need to maintain your on-premises email organization or don't plan on implementing a single sign-on solution, you can uninstall Exchange from your servers and remove your on-premises Exchange organization.

   For more information, see the following topics:

   - [Modify or Remove Exchange 2010](https://docs.microsoft.com/previous-versions/office/exchange-server-2010/ee332361(v=exchg.141))

   - [How to Remove an Exchange 2007 Organization](https://docs.microsoft.com/previous-versions/office/exchange-server-2007/aa998313(v=exchg.80))

   - [How to Uninstall Exchange Server 2003](https://docs.microsoft.com/previous-versions/tn-archive/bb125110(v=exchg.65))

   > [!NOTE]
   > Decommissioning Exchange can have unintended consequences. Before decommissioning your on-premises Exchange organization, we recommend that you contact Microsoft Support.

## See also

[Ways to migrate email to Microsoft 365 or Office 365](mailbox-migration.md)

[Decide on a migration path](decide-on-a-migration-path.md)
