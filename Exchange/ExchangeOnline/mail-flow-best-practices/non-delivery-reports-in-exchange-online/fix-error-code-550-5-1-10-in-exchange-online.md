---
title: "Fix email delivery issues for error code 550 5.1.10 in Exchange Online"
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer: 
audience: Admin
ms.topic: troubleshooting
ms.service: o365-administration
localization_priority: Priority
f1.keywords:
- CSH
ms.custom: MiniMaven
search.appverid:
- BCS160
- MOE150
- MET150
ms.assetid: 5a04a25a-a34f-476b-afc6-007fb92f86a1
description: "Learn how to fix email issues for error code 550 5.1.10 in Exchange Online (invalid recipient or backscatter)."
---

# Fix email delivery issues for error code 550 5.1.10 in Exchange Online

Problems sending and receiving email messages can be frustrating. If you get a non-delivery report (NDR), also called a bounce message, for error code 550 5.1.10, this article can help you fix the problem and get your message sent.

![Email user icon](../../media/31425afd-41a9-435e-aa85-6886277c369b.png) [I got this bounce message. How do I fix it?](#i-got-this-bounce-message-how-do-i-fix-it)

![Email admin icon](../../media/3d4c569e-b819-4a29-86b1-4b9619cf2acf.png) [I'm an email admin. How can I fix this?](#im-an-email-admin-how-can-i-fix-this)

## Why did I get this bounce message?

You received this NDR with error code 5.1.10 for one of the following reasons:

- The recipient's email address doesn't exist or couldn't be found. Go to the [I got this bounce message. How do I fix it?](#i-got-this-bounce-message-how-do-i-fix-it) section in this topic.

Typically, if a message can't be delivered, the recipient's email system will use the sender's email address in the **From** field to notify the sender in an NDR like this one. But what if the message was sent by a spammer who falsified the **From** address so it appears the message came from your email address? The resulting NDR that you'll receive is useless because it creates the false impression that you did something wrong. This type of useless NDR is called _backscatter_. It's annoying, but if this NDR is backscatter, your account hasn't been compromised.

- A spammer sent a message to a non-existent recipient, and they falsified the **From** address so it appears the message was sent by your email address. The resulting bounce message that you get is called _backscatter_, and you can safely ignore or delete the bounce message.

  Backscatter itself is harmless, but if you're getting a lot of it, it's possible that your computer or device is infected with spam-sending malware. Consider running an anti-malware scan. Additionally, to help prevent spammers from impersonating you or others in your organization, ask your email admin to read this topic: [Set up SPF to help prevent spoofing](https://docs.microsoft.com/microsoft-365/security/office-365-security/set-up-spf-in-office-365-to-help-prevent-spoofing).

## I got this bounce message. How do I fix it?

Here are some steps that you can try to fix the problem yourself.

If the steps in this section don't fix the problem for you, contact your email admin and refer them to the information in this topic so they can try to resolve the issue for you.

### Verify recipient's email address and resend your message

#### Verify recipient's email address and resend your message in Outlook

1. Open the bounce message. In the **Report** tab, choose **Send Again**.

   ![Screenshot shows the Report tab of a bounce message with the Send Again option and text in the body of the email message that says the message couldn't be delivered.](../../media/ecbdcf67-a13e-4d6c-bc56-ecd5c7ce9a4e.png)

   If your original message had an attachment larger than 10 MB, the **Send Again** option might not be available or might not work. Instead, resend the message from your **Sent Items** folder. For more information, see [Resend an email message](https://support.office.com/article/acd16ac4-c881-477d-b4aa-36168fa96088.aspx).

2. In the new copy of your message, select the recipient's email address in the **To** box and then press the **Delete** key.

3. Remove the recipient's email address from the Auto-Complete list (a bad or outdated entry could be causing the problem):

   1. In the **To** box, start typing the recipient's email address until it appears in the Auto-Complete drop-down list as shown below.

      ![Screenshot shows the Send Again option for an email message. In the Resend to field, the AutoComplete feature provides the email address for the recipient based on the first few letters typed of the recipient's name.](../../media/409b56f4-1a7f-417e-94cf-9a058ac851d4.png)

   2. Use the Down Arrow key to select the recipient from the Auto-Complete drop-down list and then press the Delete key or choose the **Delete** icon ![Delete icon](../../media/c12e6d18-953d-4955-8c7b-11719aa2a326.png) to the right of the email address.

4. In the **To** box, continue typing the entire recipient email address. Be sure to spell the address correctly.

   ![Screenshot shows the Send Again option for an email message. In the Resend to field, the recipient's address has been provided by the AutoComplete feature.](../../media/0f5e6165-60bc-4dfc-b327-f844392d0022.png)

5. Click **Send**.

#### Verify recipient's email address and resend your message in Outlook on the web (formerly known as Outlook Web App)

1. Open the bounce message. In the reading pane, just below the message header information, choose **To send this message again, click here**.

   ![Screenshot shows a portion of an Undeliverable bounce message with the option to send the message again.](../../media/e68ff341-84e8-4acd-8e3d-e5efb222e1fb.png)

   If your original message had an attachment larger than 10 MB, the **Send Again** option might not be available or might not work. Instead, resend the message from your **Sent Items** folder.

2. On the **To** line of the new copy of your message, choose the **Delete** icon ![Delete icon](../../media/c12e6d18-953d-4955-8c7b-11719aa2a326.png) delete the recipient's email address.

   ![Screenshot shows the To line of an email message with the option to delete the recipient's email address.](../../media/09ddb461-f132-48c0-b7b8-d856163f1820.png)

3. Remove the recipient's email address from the Auto-Complete list (a bad or outdated entry could be causing the problem):

   1. On the empty **To** line, start typing the recipient's name or email address until it appears in the Auto-Complete drop-down list.

      ![Screenshot shows the To line of an email message with the option to delete the recipient's email address from the Auto-Complete list.](../../media/dc67e6ed-35e1-4085-adc7-8997ea155070.png)

   2. Use the Down Arrow key to select the recipient from the Auto-Complete list, and then press the Delete key. Or, hover over the recipient's name and click the **Delete** icon ![Delete icon](../../media/c12e6d18-953d-4955-8c7b-11719aa2a326.png).

4. On the **To** line, continue typing the recipient's entire email address. Be sure to spell the address correctly.

5. Click **Send**.

### Ask the recipient to check for broken forwarding rules or settings

Does the recipient's email address in your original message exactly match the recipient's email address in the NDR? Compare the recipient's email address in the NDR with the recipient's email address in the message in your **Sent Items** folder.

If the addresses don't match, contact the recipient (by phone, in person, etc.) and ask them if they've configured an email rule that forwards incoming email messages from you to another destination. Their rule could have tried to send a copy of your message to a bad email address. If the recipient has such a rule, they'll need to correct the destination email address or remove the rule in order to prevent 5.1.x message delivery errors.

Office 365 supports multiple ways to forward messages automatically. If the intended recipient of your message is using Office 365, ask them to review the [Update, disable, or remove Inbox Rules forwarding](#update-disable-or-remove-inbox-rules-forwarding) and [Disable account forwarding](#disable-account-forwarding) sections below.

If the problem persists after performing these steps, ask the recipient to refer their email admin to the [I'm an email admin. How can I fix this?](#im-an-email-admin-how-can-i-fix-this) section below.

#### Update, disable, or remove Inbox Rules forwarding

1. In Office 365, sign in to your user account.

2. Click the gear icon in the top right corner to show the **Settings** pane.

3. Select **Your app settings** \> **Mail**.

    ![Screenshot shows the Settings pane with the Mail option highlighted in the Your app settings section.](../../media/06f81ddb-40bb-43fb-b2ce-0a221c67960e.png)

4. From the **Options** navigation pane on the left, select **Mail** \> **Automatic processing** \> **Inbox and sweep rules**.

    ![Shows the Inbox rules area of the Inbox and sweep rules option in the Options for Mail in Exchange Online. You can create, edit, and delete inbox rules to handle your email.](../../media/dc0e48ef-1d9f-4bd4-8c49-b69a4b988541.png)

5. Update, turn off, or delete any rules that might be forwarding the sender's message to a non-existent or broken email address.

#### Disable account forwarding

1. Sign in to your Office 365 account, and from the same **Options** navigation as shown above, select **Mail** \> **Accounts** \> **Forwarding**.

    ![Screenshot shows the Forwarding option page with the Stop forwarding option selected.](../../media/5423f042-6cba-465d-b1cd-4ef1051eb9ed.png)

2. Select **Stop forwarding** and click **Save** to disable account forwarding.

## I'm an email admin. How can I fix this?

If the sender can't fix the issue themselves, the problem might be that an email system on the receiving side isn't configured correctly. If you're the email admin for the recipient, try one or more of the following fixes and then ask the sender to resend the message.

### Verify that the recipient exists and has an active license assigned

To verify that the recipient exists and has an active license assigned:

1. In the Microsoft 365 admin center, choose **Users** to go to the **Active users** page.

2. In the **Active users** \> **Filters** search field, type part of the recipient's name, and then press Enter to locate the recipient. If the recipient doesn't exist, then you must create a new mailbox or contact for this user. (For more information, see [Add users individually or in bulk to Office 365 - Admin Help](https://docs.microsoft.com/microsoft-365/admin/add-users/add-users).) If the recipient does exist, make sure the recipient's username matches the email address the sender used.

   ![Screenshot shows a section of the Active users page with a search term, "allie", typed in the search box adjacent to the Filters option, which is set to All users. Below, the complete display name and username are shown.](../../media/4b17dfe8-104c-4e8a-9325-3779a7d4bc5f.png)

3. If the user's mailbox is hosted in Exchange Online, click the user's record to review their details and verify that they've been assigned a valid license for email (for example, an Office 365 Enterprise E5 license).

   ![Screenshot shows information for the user named Allie Bellew. The Product licenses area shows that no products have been assigned for the user and the option to edit is available.](../../media/c622fca5-7163-412a-b228-20d411c5bdf2.png)

4. If the user's mailbox is hosted in Exchange Online, but no license has been assigned, choose **Edit** and assign the user a license.

   ![Shows an Office 365 Enterprise E5 license with 12 licenses available.](../../media/8e0f0507-d9bf-4559-bc93-6f907e04e74a.png)

### Fix or remove broken forwarding rules or settings

Office 365 provides the following features for users and email admins to forward messages to another email address:

- Forwarding using Inbox rules (user)

- Account forwarding (user and email admin)

- Forwarding using mail flow rules (email admin)

Follow the steps below to fix the recipient's broken mail forwarding rule or settings.

#### Forwarding using Inbox rules (user)

The recipient might have an Inbox rule that is forwarding messages to a problematic email address. Inbox rules are available only to the user (or someone with delegated access to their account). See [Update, disable, or remove Inbox Rules forwarding](#update-disable-or-remove-inbox-rules-forwarding) for how the user, or their delegate, can change or remove a broken forwarding Inbox rule.

#### Account forwarding (user and email admin)

1. In the Microsoft 365 admin center, choose **Users**.

2. In the **Active users** \> **Filters** search field, type part of the recipient's name and then press Enter to locate the recipient. Click the user's record to view its details.

3. From the user's profile page, select **Mail Settings** \> **Email forwarding** \> **Edit**.

   ![Screenshot shows the user profile page for the user named Allie Bellew with  Email forwarding  set to Applied and an edit option available.](../../media/5762c3b7-5336-47c6-8a54-1b06fbff32c5.png)

4. Turn off **Email forwarding** and select **Save**.

   ![Screenshot shows the user profile page for the user named Allie Bellew with  Email forwarding  set to Applied and an edit option available.](../../media/96d62151-a740-414f-97b3-57b3e32ad76e.png)

#### Forwarding using mail flow rules (email admin)

Unlike Inbox rules which are associated with a user's mailbox, mail flow rules (also known as transport rules) are organization-wide settings and can only be created and edited by email admins.

1. In the Microsoft 365 Admin center, select **Admin centers** \> **Exchange**.

   ![Screenshot shows the admin center with the Admin centers option expanded and Exchange selected.](../../media/47399df2-0bc4-42e2-b183-07750a46bc68.png)

2. In the Exchange admin center (EAC), go to **Mail flow** \> **Rules**.

3. Look for any redirect rules that might be forwarding the sender's message to another address. An example is shown below.

   ![Screenshot shows the Rules page of the Mail flow area in the Exchange admin center. The On check box is selected for the Rule to redirect user Allie Bellew's mail.](../../media/de24b162-f8bc-416b-8a1f-4ad58c9f52c2.png)

4. Update, turn off, or delete any suspect forwarding rules.

#### Update accepted domain settings

**Notes**:

- Message routing (especially in hybrid configurations) can be complex. Even if changing the accepted domain setting fixes the bounce message problem, it might not be right solution for you. In some cases, changing the accepted domain type might cause other unanticipated problems. Review [Manage accepted domains in Exchange Online](../manage-accepted-domains/manage-accepted-domains.md) and then proceed with caution.

  - **If the accepted domain in Exchange Online is Authoritative**: The service looks for the recipient in the Exchange Online organization, and if the recipient isn't found, message delivery stops and the sender will receive this bounce message. On-premises users must be represented in the Exchange Online organization by mail contacts or mail users (created manually or by directory synchronization).

  - **If the accepted domain in Exchange Online is Internal Relay**: The service looks for the recipient in the Exchange Online organization, and if the recipient isn't found, the service relays the message to your on-premises Exchange Organization (assuming you've correctly set up the required connector to do so).

- When setting an accepted domain to Internal Relay, you must set up a corresponding Office 365 connector to your on-premises environment. Failing to do so will break mail flow to your on-premises recipients. For more information about connectors, see [Configure mail flow using connectors in Office 365](https://docs.microsoft.com/Exchange/mail-flow-best-practices/use-connectors-to-configure-mail-flow/use-connectors-to-configure-mail-flow).

**To change the Accepted Domain from Authoritative to Internal Relay**

If you have a hybrid configuration with an Office 365 connector configured to route messages to your on-premises environment, and you believe that Internal Relay is the correct setting for your domain, change the Accepted Domain from Authoritative to Internal Relay.

1. Open the Exchange admin center (EAC). For more information, see [Exchange admin center in Exchange Online](https://docs.microsoft.com/Exchange/exchange-admin-center).

2. From the EAC, choose **Mail flow** \> **Accepted domains** and select the recipient's domain.

   ![Screenshot shows the Accepted Domains page of the Exchange admin center. Information about the name, accepted domain, and domain type is shown.](../../media/3d794bb4-09a7-4725-96d8-86671dad28ef.png)

3. Double-click the domain name.

4. In the **Accepted Domain** dialog box, set the domain to **Internal Relay**, and then select **Save**.

   ![Screenshot shows the Accepted Domain dialog with the Internal Relay option selected for the specified accepted domain.](../../media/0d2d80b9-66b8-474e-851b-700127c1c1d0.png)

#### Manually synchronize on-premises and Office 365 directories

If you have a hybrid configuration and the recipient is located in the on-premises Exchange organization, it's possible that the recipient's email address isn't properly synchronized with Office 365. Follow these steps to synchronize directories manually:

1. Log into the on-premises server that's running the Directory Synchronization Tools.

2. Open Windows PowerShell on the server and run the following commands:

   ```powershell
   CD "C:\Program Files\Microsoft Online Directory Sync"
   ```

   ```powershell
   DirSyncConfigShell.psc1
   ```

   ```powershell
   Start-OnlineCoexistenceSync
   ```

When synchronization completes, repeat the steps in the [Verify that the recipient exists and has an active license assigned](#verify-that-the-recipient-exists-and-has-an-active-license-assigned) section to verify that the recipient address exists in Exchange Online.

## Verify the custom domain's mail exchanger (MX) record

If you have a custom domain (for example, contoso.com instead of contoso.onmicrosoft.com), it's possible that your domain's MX record isn't configured correctly.

1. In the Microsoft 365 Admin center, go to **Settings** \> **Domains**, and then select the recipient's domain.

   ![Screenshot shows admin center with the Domains option selected. Domain names are shown on the page along with the options to add or buy a domain.](../../media/2cbbe5c3-9d92-4d27-84bc-04e2e78caaf2.png)

2. In the pop-out **Required DNS settings** pane, select **Check DNS**.

   ![Screenshot shows the Required DNS settings page and the Check DNS button is the focus.](../../media/5fea3b58-6016-4c91-92e8-229b59b47b4d.png)

3. Verify that there's only one MX record configured for the recipient's domain. Microsoft doesn't support using more than one MX record for a domain that's enrolled in Exchange Online.

4. If Office 365 detects any issues with your Exchange Online DNS record settings, follow the recommended steps to fix them. You might be prompted to make the changes directly within the Microsoft 365 admin center. Otherwise, you must update the MX record from your DNS host provider's portal. For more information, see [Create DNS records at any DNS hosting provider](https://docs.microsoft.com/microsoft-365/admin/get-help-with-domains/create-dns-records-at-any-dns-hosting-provider).

   > [!NOTE]
   > Typically, your domain's MX record should point to the Office 365 fully qualified domain name: \<your domain\>.mail.protection.outlook.com. DNS record updates usually propagate across the Internet in a few hours, but they can take up to 72 hours.

## Still need help with a 5.1.10 bounce message?

[![Get help from the Office 365 community forums](../../media/12a746cc-184b-4288-908c-f718ce9c4ba5.png)](https://go.microsoft.com/fwlink/p/?LinkId=518605)

[![Admins: Sign in and create a service request](../../media/10862798-181d-47a5-ae4f-3f8d5a2874d4.png)](https://admin.microsoft.com/AdminPortal/Home#/support

[![Admins: Call Support](../../media/9f262e67-e8c9-4fc0-85c2-b3f4cfbc064e.png)](https://go.microsoft.com/fwlink/p/?LinkID=518322)

## See also

[Email non-delivery reports in Exchange Online](non-delivery-reports-in-exchange-online.md)

[Backscatter in EOP](https://docs.microsoft.com/microsoft-365/security/office-365-security/backscatter-messages-and-eop)

[Configure email forwarding for a mailbox](../../recipients-in-exchange-online/manage-user-mailboxes/configure-email-forwarding.md)

[Synchronizing your directory with Office 365 is easy](https://go.microsoft.com/fwlink/p/?linkid=833532)

[Create DNS records at any DNS hosting provider](https://docs.microsoft.com/microsoft-365/admin/get-help-with-domains/create-dns-records-at-any-dns-hosting-provider)

[Set up SPF to help prevent spoofing](https://docs.microsoft.com/microsoft-365/security/office-365-security/set-up-spf-in-office-365-to-help-prevent-spoofing)
