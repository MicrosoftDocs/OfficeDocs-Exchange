---
title: "Move mailboxes between on-premises and Exchange Online organizations in hybrid deployments"
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
audience: ITPro
ms.topic: article
f1.keywords:
- CSH
ms.custom:
- 'O365E_HRCMoveRequest_FL312271'
ms.prod: exchange-server-it-pro
localization_priority: Normal
ms.collection:
- Hybrid
- Ent_O365_Hybrid
- M365-email-calendar
- M365-email-calendar
ms.assetid: d6289f7b-f67e-48db-9570-9fd3c9547548
ms.reviewer:
description: "With an Exchange-based hybrid deployment, you can choose to either move on-premises Exchange mailboxes to the Exchange Online organization or move Exchange Online mailboxes to the Exchange organization. When you move mailboxes between the on-premises and Exchange Online organizations, you use migration batches to perform the remote mailbox move request. This approach allows you to move existing mailboxes instead of creating new user mailboxes and importing user information. This approach is different than migrating user mailboxes from an on-premises Exchange organization to Exchange Online as part of a complete Exchange migration to the cloud. The mailbox moves discussed in this topic are part of administrative Exchange management in a longer-term coexistence relationship between on-premises Exchange and Exchange Online organizations."
---

# Move mailboxes between on-premises and Exchange Online organizations in hybrid deployments

With an Exchange-based hybrid deployment, you can choose to either move on-premises Exchange mailboxes to the Exchange Online organization or move Exchange Online mailboxes to the Exchange organization. When you move mailboxes between the on-premises and Exchange Online organizations, you use migration batches to perform the remote mailbox move request. This approach allows you to move existing mailboxes instead of creating new user mailboxes and importing user information. This approach is different than migrating user mailboxes from an on-premises Exchange organization to Exchange Online as part of a complete Exchange migration to the cloud. The mailbox moves discussed in this topic are part of administrative Exchange management in a longer-term coexistence relationship between on-premises Exchange and Exchange Online organizations.

For more information about migrating on-premises Exchange organizations to Exchange Online, see [Ways to migrate multiple email accounts to Microsoft 365 or Office 365](https://docs.microsoft.com/Exchange/mailbox-migration/mailbox-migration).

> [!IMPORTANT]
> You must have configured a hybrid deployment between your on-premises and Exchange Online organizations to complete the mailbox moves procedures in this topic. For more information about hybrid deployments, see [Exchange Server Hybrid Deployments](../exchange-hybrid.md).

> [!IMPORTANT]
> Before you move Unified Messaging-enabled (UM) mailboxes to Exchange Online, you need to make sure that on-premises Skype for Business 2015, Skype for Business Online, and Exchange Online, all meet the requirements specified in [Hybrid deployment prerequisites](../hybrid-deployment-prerequisites.md). For information on how to map your on-premises UM mailbox policies to policies in Exchange Online, see [Set-UMMailboxPolicy](https://docs.microsoft.com/powershell/module/exchange/set-ummailboxpolicy).

## What do you need to know before you begin?

- Estimated time to complete: 10 minutes to configure the migration batch, but total time to complete the migration depends on the number of mailboxes included in each migration batch.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox Move and Migration Permissions" section in the [Recipients permissions](https://docs.microsoft.com/Exchange/permissions/feature-permissions/recipient-permissions) topic.

- Hybrid deployment is configured between your on-premises and Exchange Online organizations.

- If you're running Exchange 2013, make sure the Mailbox Replication Proxy Service (MRSProxy) is enabled on your on-premises Exchange 2013 Client Access servers.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](https://docs.microsoft.com/Exchange/accessibility/keyboard-shortcuts-in-admin-center).

- The Microsoft 365 or Office 365 Exchange license must be assigned only after the migration is complete. You then have 30 days to assign the license.

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612),[Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Step 1: Create a migration endpoint
<a name="bkmk_create"> </a>

Prior to performing on-boarding and off-boarding remote move migrations in an Exchange hybrid deployment, we recommend that you create Exchange remote migration endpoints. The migration endpoint contains the connection settings for an on-premises Exchange server that is running the MRS proxy service, which is required to perform remote move migrations to and from Exchange Online.

## Step 2: Enable the MRSProxy service
<a name="bkmk_enable"> </a>

If the MRSProxy service isn't already enabled for your on-premises Exchange servers, follow these steps in the Exchange admin center (EAC):

1. Open the EAC, and then navigate to **Servers** \> **Virtual Directories**.

2. Select the Client Access server, and then select the **EWS** virtual directory and click **Edit** ![Edit icon](../media/ITPro_EAC_EditIcon.gif).

3. Select the **MRS Proxy enabled** check box, and then click **Save**.

## Step 3: Use the EAC to move mailboxes
<a name="bkmk_eac"> </a>

You can use the remote move migration wizard on the **Office 365** tab in the EAC on an Exchange server to either move existing user mailboxes in the on-premises organization to the Exchange Online organization or to move Exchange Online mailboxes to the on-premises organization. Choose one of the following procedures:

### Move on-premises mailboxes to Exchange Online
<a name="bkmk_step3Move"> </a>

You can use the remote move migration wizard in the EAC in the on-premises Exchange organization to move existing user mailboxes from on-premises Exchange to Exchange Online:

1. In the EAC, go to **Office 365** \> **Recipients** \> **Migration**.

2. Click **Add** ![Add Icon](../media/ITPro_EAC_AddIcon.gif), and then select **Migrate to Exchange Online**.

3. On the **Select a migration type** page, select **Remote move migration** and then click **Next**.

4. On the **Select the users** page, click **Add** ![Add Icon](../media/ITPro_EAC_AddIcon.gif) and select the on-premises users to move to Microsoft 365 or Office 365 and click **Add** and then click **OK**. Click **Next**.

   > [!NOTE]
   > If shared mailbox accounts don't appear in the user selection list, you need to sync the shared mailbox on-premises AD accounts to Microsoft 365 or Office 365 by using Azure AD Connect. The shared mailbox AD accounts will appear as blocked accounts in the Microsoft 365 admin center and you will be able to select them from the user list.```

5. On the **Enter the Windows user account credential** page, enter the on-premises administrator account name in the **On-premises administrator name** text field and enter the associated password for this account in the **On-premises administrator password** text field. For example, "corp\administrator" and a password. Click **Next**.

    > [!NOTE]
    > If you've already created a migration endpoint, you'll receive an endpoint confirmation prompt for this step. If you've created two or more migration endpoints, you must choose an endpoint from the migration endpoint drop-down menu.

6. On the **Confirm the migration endpoint** page, verify that the FQDN of your on-premises Exchange server is listed when the wizard confirms the migration endpoint. For example, "mail.contoso.com". Click **Next**.

    > [!NOTE]
    > The MRSProxy service on the Exchange servers automatically throttles the mailbox move requests when you select multiple mailboxes to move to Exchange Online. The total time to complete the mailbox move depends on the total number of mailboxes selected, the size of the mailboxes, and the configuration of the MRSProxy. To learn more about customizing the MRSProxy, see [Message Throttling](https://docs.microsoft.com/exchange/mail-flow/message-rate-limits).

7. On the **Move configuration** page, enter a name for the migration batch in the **New migration batch name** text field. Use the down arrow ![Down Arrow Icon](../media/ITPro_EAC_DownArrowIcon.gif) to select the **Target delivery domain for the mailboxes that are migrating to Office 365**. In most hybrid deployments, this is the primary SMTP domain used for the Exchange Online organization mailboxes. For example, contoso.mail.onmicrosoft.com. Verify that the **Move primary mailbox along with archive mailbox** option is selected, and then click **Next**.

8. On the **Start the batch** page, select at least one recipient to receive the batch complete report. Verify that the **Automatically start the batch** option is selected, and then select the **Automatically complete the migration batch** check box. Click **New**.

    > [!NOTE]
    > If you select **Manual Complete the batch** in step **8**, Exchange Online will only synchronize 95% of each mailbox in that batch. Exchange Online will periodically synchronize the batch to keep each mailbox at 95% synchronization until the batch is manually completed by clicking the **Complete this migration batch**, at which point the remaining 5% is migrated.

### Move Exchange Online mailboxes to the on-premises organization
<a name="bkmk_eacExo"> </a>

You can use the remote move migration wizard in the EAC in Exchange Online to move existing cloud mailboxes to the on-premises Exchange organization:

1. In the EAC, go to **Office 365** \> **Recipients** \> **Migration**.

2. Click **Add** ![Add Icon](../media/ITPro_EAC_AddIcon.gif), and then select **Migrate from Exchange Online**.

3. On the **Select the users** page, select **Select the users that you want to move** and then click **Next**.

4. On the **Select the users** page, click **Add** ![Add Icon](../media/ITPro_EAC_AddIcon.gif) and then select the Exchange Online users to move to the on-premises organization, click **Add** and then click **OK**. Click **Next**.

5. On the **Confirm the migration endpoint** page, verify that the FQDN of your on-premises Exchange server is listed when the wizard confirms the migration endpoint. For example, "mail.contoso.com". Click **Next**.

    > [!NOTE]
    > The MRSProxy service on the Exchange servers automatically throttles the mailbox move requests when you select multiple mailboxes to move to Exchange Online. The total time to complete the mailbox move depends on the total number of mailboxes selected, the size of the mailboxes, and the properties of the MRSProxy. To learn more about customizing the MRSProxy, see [Message Throttling](https://docs.microsoft.com/exchange/mail-flow/message-rate-limits).

6. On the **Move configuration** page, enter a name for the migration batch in the **New migration batch name** text field. Then enter the target delivery domain in the **Target delivery domain for the mailboxes that are migrating to Office 365** field. In most hybrid deployments, this is the primary SMTP domain used for both on-premises and Exchange Online organization mailboxes. For example, contoso.com.

7. Choose whether to also move the archive mailbox for the selected user and enter the database name you'd like to move this mailbox to in the **Target database** text field. For example, Mailbox Database 123456789. Click **Next**.

8. On the **Start the batch** page, select at least one recipient to receive the batch complete report. Verify that **Automatically start the batch** is selected, and then select the **Automatically complete the migration batch** check box. Click **New**.

## Step 4: Remove completed migration batches
<a name="bkmk_remove"> </a>

After your mailbox moves have completed, we recommend removing the completed migration batches to minimize the likelihood of errors if the same users are moved again.

To remove a completed migration batch:

1. Open the EAC and navigate to **Office 365** \> **Recipients** \> **Migrations**.

2. Click a completed migration batch, and then click **Delete** ![Delete icon](../media/ITPro_EAC_DeleteIcon.gif).

3. On the deletion warning confirmation dialog, click **Yes**.

## Step 5: Re-enable offline access for Outlook on the web
<a name="bkmk_step5owa"> </a>

Offline access in Outlook on the web (formerly called Outlook Web App) lets users access their mailbox when they're not connected to a network. If you migrate Exchange mailboxes to Exchange Online, users have to reset the offline access setting in their browser to use Outlook on the web offline. For more information about offline access in Outlook on the web, the browsers that support it, and how to turn it on, see [Using Outlook Web App offline](https://support.microsoft.com/office/3214839c-0604-4162-8a97-6856b4c27b36).

## How do you know this worked?
<a name="bkmk_how"> </a>

When you move existing user mailboxes between the on-premises and Exchange Online organizations, the successful completion of the remote move wizard will be your first indication that moving the mailbox will complete as expected.

Because the mailbox move process takes several minutes to complete, you can also verify that the move is working correctly by opening the EAC and selecting **Office 365** \> **Recipients** \> **Migration** to display the move status for the mailboxes selected in the remote move wizard. The value of the **Status** is **Syncing** during the mailbox move, and it's **Completed** when the mailbox has successfully moved to either the on-premises or Exchange Online organization.

After the mailbox move has completed, you can check that the remote mailbox located on the on-premises or Exchange Online organization has been successfully moved by verifying the mailbox properties. To do this, navigate to **Recipients** \> **Mailboxes** in the EAC for either the on-premises organization or Exchange Online organization. The user mailbox should show a **Mailbox Type** of **Office 365** for Exchange Online mailboxes and **User** for on-premises mailboxes.

You can also run the following cmdlet in the Exchange Management Shell to verify the status of the migration batch.

```PowerShell
Get-MigrationBatch -Identity <batch name>
```

Having problems? Ask for help in the Microsoft 365 and Office forums. To access the forums, you'll need to sign in using an account that's granted administrator access to your cloud-based service. Visit the forums at: [Microsoft 365 and Office Forums](https://answers.microsoft.com/msoffice/forum)
