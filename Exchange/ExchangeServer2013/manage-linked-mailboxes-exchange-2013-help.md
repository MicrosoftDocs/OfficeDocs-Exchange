---
title: 'Manage linked mailboxes: Exchange 2013 Help'
TOCTitle: Manage linked mailboxes
ms:assetid: 76e12d4a-1c3a-42e2-b64c-c09d36e81bd3
ms:mtpsurl: https://technet.microsoft.com/library/JJ673532(v=EXCHG.150)
ms:contentKeyID: 49315442
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Manage linked mailboxes

_**Applies to:** Exchange Server 2013_

Linked mailboxes are mailboxes that are accessed by users in a separate, trusted forest. Linked mailboxes may be necessary for organizations that deploy Exchange in a resource forest. The resource forest scenario allows an organization to centralize Exchange in a single forest, while allowing access to the Exchange organization with user accounts that are located in one or more trusted forests (called *account forests*). The user account that accesses the linked mailbox doesn't exist in the forest where Exchange is deployed. Therefore, a disabled user account that exists in the same forest as Exchange is created and associated with the corresponding linked mailbox.

The following figure illustrates the relationship between the linked user account used to access the linked mailbox (located in the account forest) and the disabled user account in the Exchange resource forest that's associated with the linked mailbox.

**Linked mailboxes**

![Complex Exchange organization with resource forest](images/Aa998031.706725cf-e520-4b89-a275-acd8fb58943a(EXCHG.150).gif "Complex Exchange organization with resource forest")

> [!NOTE]
> A trust between the Exchange forest and at least one account forest must be set up before you can create linked mailboxes. At a minimum, you must set up a one-way, outgoing trust so that the Exchange forest trusts the account forest. For more information, see <A href="https://docs.microsoft.com/previous-versions/exchange-server/exchange-150/jj156983(v=exchg.150)">Learn more about setting up a forest trust to support linked mailboxes</A>.

## What do you need to know before you begin?

- Estimated time to complete: 2 to 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipient Provisioning Permissions" section in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- A user account (called the *linked master account*) must exist in the account forest before you can create a linked mailbox. This is because the linked mailbox is associated with a user in the account forest.

- If you've configured a one-way outgoing trust where the Exchange forest trusts the account forest, you'll need administrator credentials in the account forest to create a linked mailbox.

   To create a linked mailbox without being prompted for administrator credentials in the account forest, you have to create a two-way trust, or create another one-way outgoing trust where the account forest also trusts the Exchange forest. This step also requires administrator credentials in the account forest.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Create a linked mailbox

## Use the EAC to create a linked mailbox

1. In the EAC, navigate to **Recipients**  \> **Mailboxes**.

2. Click **New** \> **Linked mailbox**.

3. On the **New linked mailbox** page, in the **Trusted forest or domain** box, select the name of the account forest that contains the user account that you're creating the linked mailbox for. Click **Next**.

4. If your organization has configured a one-way outgoing trust where the Exchange forest trusts the account forest, you're prompted for administrator credentials in the account forest so that you can gain access to a domain controller in the trusted forest. Type the username and password for an administrator account in the account forest, and then click **Next**.

    > [!NOTE]
    > You won't be prompted for administrator credentials if you've created a two-way trust or have created another one-way outgoing trust where the account forest trusts the Exchange forest.

5. Complete the following boxes on the **Select linked master account** page.

   - **Linked domain controller**: Select a domain controller in the account forest. Exchange will connect to this domain controller to retrieve the list of user accounts in the account forest so that you can select the linked master account.

   - **Linked master account**: Click **Browse**, select a user account in the account forest, and then click **OK**. The new linked mailbox will be associated with this account.

6. Click **Next** and complete the following boxes on the **Enter general information** page.

   - **&#42; Name**: Use this box to type a name for the user. This is the name used as the display name in the EAC and your organization's address book, and the name that's listed in Active Directory. This name is required.

   - **Organizational unit**: You can select an organizational unit (OU) other than the default (which is the recipient scope). If the recipient scope is set to the forest, the default value is set to the Users container in the Active Directory domain that contains the computer on which the EAC is running. If the recipient scope is set to a specific domain, the Users container in that domain is selected by default. If the recipient scope is set to a specific OU, that OU is selected by default.

     To select a different OU, click **Browse**. The dialog box displays all OUs in the Exchange forest that are within the specified scope. Select the OU you want, and then click **OK**.

   - **&#42; User logon name**: Use this box to type the user logon name, which is required to create a linked mailbox. Type the username here. This name will be used in the left portion of the email address for the linked mailbox if you don't specify an alias.

     > [!NOTE]
     > Because the user account that is created in the Exchange forest is disabled when you create a linked mailbox, the user doesn't use the user logon name to sign in to the linked mailbox. They sign in using their credentials from the account forest.

7. Click **More options** to configure the following boxes. Otherwise, skip to Step 8 to save the new linked mailbox.

   - **Alias**: Type the alias, which specifies the email alias for the linked mailbox. The user's alias is the portion of the email address on the left side of the at (@) symbol. It must be unique in the forest.

     > [!NOTE]
     > If you leave this box blank, the value from the username portion of the <STRONG>User Logon Name</STRONG> is used for the email alias.

   - **First name**, **Initials**, **Last name**

   - **Mailbox database**: Use this option to specify a mailbox database instead of allowing Exchange to choose a database for you. Click **Browse** to open the **Select Mailbox Database** dialog box. This dialog box lists all the mailbox databases in your Exchange organization. By default, the mailbox databases are sorted by name. You can also click the title of the corresponding column to sort the databases by server name or version. Select the mailbox database you want to use, and then click **OK**.

   - **Address book policy**: Use this option to specify an address book policy (ABP) for the linked mailbox. ABPs contain a global address list (GAL), an offline address book (OAB), a room list, and a set of address lists. When assigned to users, an ABP provides them with access to a customized GAL in Outlook and Outlook Web App. To learn more, see [Address book policies](https://docs.microsoft.com/exchange/address-books/address-book-policies/address-book-policies).

     In the drop-down list, select the policy that you want associated with this mailbox.

8. When you're finished, click **Save** to create the new linked mailbox.

## Use the Shell to create a linked mailbox

This example creates a linked mailbox for Ayla Kol in the CONTOSO Exchange resource forest. The FABRIKAM domain is in the account forest. The administrator account FABRIKAM \\administrator is used to access the linked domain controller.

```powershell
New-Mailbox -Name "Ayla Kol" -LinkedDomainController "DC1_FABRIKAM" -LinkedMasterAccount " FABRIKAM\aylak" -OrganizationalUnit Users -UserPrincipalName aylak@contoso.com -LinkedCredential:(Get-Credential FABRIKAM\administrator)
```

For syntax and parameter information, see [New-Mailbox](https://docs.microsoft.com/powershell/module/exchange/New-Mailbox).

## How do you know this worked?

To verify that you've successfully created a linked mailbox, do one of the following:

- In the EAC, navigate to **Recipients**  \> **Mailboxes**. The new linked mailbox is displayed in the mailbox list. Under **Mailbox Type**, the type is **Linked**.

- In the Shell, run the following command to display information about the new linked mailbox.

  ```powershell
  Get-Mailbox <Name> | FL Name,RecipientTypeDetails,IsLinked,LinkedMasterAccount
  ```

## Change linked mailbox properties

After you create a linked mailbox, you can make changes and set additional properties by using the Exchange admin center (EAC) or the Exchange Management Shell.

You can also change properties for multiple linked mailboxes at the same time. For more information, see the section, "Bulk edit user mailboxes" section in the [Manage user mailboxes](https://docs.microsoft.com/exchange/recipients-in-exchange-online/manage-user-mailboxes/manage-user-mailboxes) topic.

> [!IMPORTANT]
> The estimated time to complete this task will vary based on the number of properties you want to view or change.

## Use the EAC to change linked mailbox properties

1. In the EAC, navigate to **Recipients**  \> **Mailboxes**.

2. In the list of mailboxes, click the linked mailbox that you want to change the properties for, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. On the mailbox properties page, click one of the following sections to view or change properties.

   - General

   - Mailbox Usage

   - Email Address

   - Mailbox Features

   - Member Of

   - MailTip

   - Mailbox Delegation

## General

Use the **General** section to view or change basic information about the user.

- **\* Linked mailbox name**: This is the name that's listed in Active Directory. If you change this name, it can't exceed 64 characters.

- **\* Display name**: This name appears in your organization's address book, on the To: and From: lines in email, and in the Mailboxes list in the EAC. This name can't contain empty spaces before or after the display name.

- **\* User logon name**    For user mailboxes, this is the name that the user uses to sign in to their mailbox and to log on to the domain. For linked mailboxes, the corresponding user account that is created in the Exchange forest when the linked mailbox was created is disabled. The user uses their credentials from the account forest to sign in to the linked mailbox.

  If you change this name, it must be unique in your organization.

- **Linked master account**: This read-only box displays the user (in the format domain\\username format) from the account forest that is associated with the linked mailbox. To change the linked master account associated with the linked mailbox, you have to use the **Set-Mailbox** cmdlet in the Shell. If you change the linked master account, the user will have to use the credentials for the new linked master account to sign in to the linked mailbox. For the command syntax to change the linked master account, see Use the Shell to change linked mailbox properties.

- **Hide from address lists**: Select this check box to prevent the linked mailbox from appearing in the address book and other address lists that are defined in your Exchange organization. After you select this check box, users can still send messages to this user by using the email address.

Click **More options** to view or change these additional properties:

- **Organizational unit**: This read-only box displays the organizational unit (OU) that contains the user account. You have to use Active Directory Users and Computers to move the user account to a different OU.

- **Mailbox database**: This read-only box displays the name of the mailbox database that hosts the mailbox. To move the mailbox to a different database, select it in the mailbox list, and then click **Move mailbox to a different database** in the Details pane.

- **\* Alias**: This specifies the email alias for the linked mailbox. The alias is the portion of the email address on the left side of the at (@) symbol. It must be unique in the forest.

- **First name**, **Initials**, **Last name**

- **Custom attributes**: This section displays the custom attributes defined for the linked mailbox. To specify custom attribute values, click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon"). You can specify up to 15 custom attributes for the recipient.

## Mailbox Usage

Use the **Mailbox Usage** section to view or change the mailbox storage quota and deleted item retention settings for the linked mailbox. These settings are configured by default when the linked mailbox is created. They use the values that are configured for the mailbox database and apply to all mailboxes in that database. You can customize these settings for each mailbox instead of using the mailbox database defaults.

- **Last logon**: This read-only box displays the last time that the user signed in to the mailbox.

- **Mailbox usage**: This area shows the total size of the mailbox and the percentage of the total mailbox quota that has been used.

> [!NOTE]
> To obtain the information that's displayed in the previous two boxes, the EAC queries the mailbox database that hosts the mailbox. If the EAC can't communicate with the Exchange store that contains the mailbox database, these boxes will be blank. A warning message is displayed if the user hasn't signed in to the mailbox for the first time.

Click **More options** to view or change the mailbox storage quota and the deleted item retention settings for the mailbox.

- **Storage quota settings**: To customize these settings for the mailbox and not use the mailbox database defaults, click **Customize settings for this mailbox**, type a new value, and then click **Save**.

   The value range for any of the storage quota settings is from 0 through 2047 gigabytes (GB).

  - **Issue a warning at (GB)**: This box displays the maximum storage limit before a warning is issued to the user. If the mailbox size reaches or exceeds the value specified, Exchange sends a warning message to the user.

  - **Prohibit send at (GB)**: This box displays the *prohibit send* limit for the mailbox. If the mailbox size reaches or exceeds the specified limit, Exchange prevents the user from sending new messages and displays a descriptive error message.

  - **Prohibit send and receive at (GB)**: This box displays the *prohibit send and receive* limit for the mailbox. If the mailbox size reaches or exceeds the specified limit, Exchange prevents the mailbox user from sending new messages and won't deliver any new messages to the mailbox. Any messages sent to the mailbox are returned to the sender with a descriptive error message.

- **Deleted item retention settings**: To customize these settings for the mailbox and not use the mailbox database defaults, click **Customize settings for this mailbox**, type a new value, and then click **Save**.

  - **Keep deleted items for (days)**: This box displays the length of time that deleted items are retained before they're permanently deleted and can't be recovered by the user. When the mailbox is created, this length of time is based on the deleted item retention settings configured for the mailbox database. By default, a mailbox database is configured to retain deleted items for 14 days. The value range for this property is from 0 through 24855 days.

  - **Don't permanently delete items until the database is backed up**: Select this check box to prevent mailboxes and email messages from being deleted until after the mailbox database on which the mailbox is located has been backed up.

## Email Address

Use the **Email address** section to view or change the email addresses associated with the linked mailbox. This includes the user's primary SMTP addresses and any associated proxy addresses. The primary SMTP address (also known as the *default reply address*) is displayed in bold text in the address list, with the uppercase **SMTP** value in the **Type** column.

- **Add**  Click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon") to add a new email address for this mailbox. Select one of following address types:

  - **SMTP**: This is the default address type. Click this radio button and then type the new SMTP address in the **\* Email address** box.

  - **EUM**: An EUM (Exchange Unified Messaging) address is used by the Microsoft Exchange Unified Messaging service to locate UM-enabled users within an Exchange organization. EUM addresses consist of the extension number and the UM dial plan for the UM-enabled user. Click this radio button and type the extension number in the **Address/Extension** box. Then click **Browse** and select a dial plan for the user.

  - **Custom address type**: Click this button and type one of the supported non-SMTP email address types in the **\* Email address** box.

    > [!NOTE]
    > With the exception of X.400 addresses, Exchange doesn't validate custom addresses for proper formatting. You must make sure that the custom address you specify complies with the format requirements for that address type.

- **Automatically update email addresses based on the email address policy applied to this recipient**: Select this check box if you want the recipient's email addresses to be updated automatically when changes are made to email address policies in your organization. This box is selected by default.

## Mailbox Features

Use the **Mailbox Features** section to view or change the following mailbox features and settings:

- **Sharing policy**: This box shows the sharing policy applied to the mailbox. A sharing policy controls how users in your organization can share calendar and contact information with users outside your Exchange organization. The Default Sharing Policy is assigned to mailboxes when they are created. To change the sharing policy that's assigned to the user, select a different one from the drop-down list.

- **Role assignment policy**: This box shows the role assignment policy assigned to the mailbox. The role assignment policy specifies the role-based access control (RBAC) roles that are assigned to the user and controls which mailbox and distribution group configuration settings users can modify. To change the role assignment policy that's assigned to the user, select a different one from the drop-down list.

- **Retention policy**: This box shows the retention policy assigned to the mailbox. A retention policy is a group of retention tags that are applied to the user's mailbox. The tags allow you to control how long to keep items in users' mailboxes and define which action to take on items that have reached a certain age. A retention policy isn't assigned to mailboxes when they are created. To assign a retention policy to the user, select one from the drop-down list.

- **Address Book policy**: This box shows the address book policy applied to the mailbox. An address book policy allows you to segment users into specific groups to provide customized views of the address book. To apply or change the address book policy that's applied to the mailbox, select one from the drop-down list.

- **Unified Messaging**: This feature is disabled by default. When you enable Unified Messaging (UM) the user will be able to use your organization's UM features and a default set of UM properties are applied to the user. Click **Enable** to enable UM for the mailbox. For information about how to enable UM, see [Enable a user for voice mail](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/set-up-voice-mail/enable-a-user-for-voice-mail).

  > [!NOTE]
  > A UM dial plan and a UM mailbox policy must exist before you can enable UM.

- **Mobile Devices**: Use this section to view and change the settings for Exchange ActiveSync, which is enabled by default. Exchange ActiveSync enables access to an Exchange mailbox from a mobile device. Click **Disable Exchange ActiveSync** to disable this feature for the mailbox.

- **Outlook Web App**: This feature is enabled by default. Outlook Web App provides access to an Exchange mailbox via a web browser. Click **Disable** to disable Outlook Web App for the mailbox. Click **Edit details** to add or change an Outlook Web App mailbox policy for the mailbox.

- **IMAP**: This feature is enabled by default. Click **Disable** to disable IMAP for the mailbox.

- **POP3**: This feature is enabled by default. Click **Disable** to disable POP3 for the mailbox.

- **MAPI**: This feature is enabled by default. MAPI enables access to an Exchange mailbox from a MAPI client such as Outlook. Click **Disable** to disable MAPI for the mailbox.

- **Litigation hold**: This feature is disabled by default. Litigation hold preserves deleted mailbox items and records changes made to mailbox items. Deleted items and all instances of changed items are returned in a discovery search. Click **Enable** to put the mailbox on litigation hold. If the mailbox is on litigation hold, click **Disable** to remove the litigation hold. If the mailbox is on litigation hold, click **Edit details** to view and change the following litigation hold settings:

  - **Hold date**: This read-only box indicates date and time when the mailbox was put on litigation hold.

  - **Put on hold by**: This read-only box indicates the user who put the mailbox on litigation hold.

  - **Note**: Use this box to notify the user about the litigation hold, explain why the mailbox is on litigation hold, or provide additional guidance to the user, such as informing them that the litigation hold won't affect their day-to-day use of email.

  - **URL**: Use this box to provide a URL to a website that provides information or guidance about the litigation hold on the mailbox.

    > [!NOTE]
    > The text from these boxes appears in the user's mailbox only if they're using Outlook 2010 or later versions. It doesn't appear in Outlook Web App or other email clients. To view the text from the Note and URL boxes in Outlook, click the <STRONG>File</STRONG> tab and, on the <STRONG>Info</STRONG> page, under <STRONG>Account Settings</STRONG>, you'll see the litigation hold comment.

- **Archiving**: If an archive mailbox doesn't exist for the user, this feature is disabled. To enable an archive mailbox, click **Enable**. If the user has an archive mailbox, the size of the archive mailbox and usage statistics are displayed. Click **Edit details** to view and change the following archive mailbox settings:

  - **Status**: This read-only box indicates whether an archive mailbox exists.

  - **Database**: This read-only box shows the name of the mailbox database that hosts the archive mailbox.

  - **Name**: Type the name of the archive mailbox in this box. This name is displayed under the folder list in Outlook or Outlook Web App.

  - **Quota usage**: This read-only area shows the total size of the archive mailbox and the percentage of the total archive mailbox quota that has been used.

  - **Quota value (GB)**: This box shows the total size of the archive mailbox. To change the size, type a new value in the box or select a value from the drop-down list.

  - **Issue warning at (GB)**: This box shows the maximum storage limit for the archive mailbox before a warning is issued to the user. If the archive mailbox size reaches or exceeds the value specified, Exchange sends a warning message to the user. To change this limit, type a new value in the box or select a value from the drop-down list.

- **Delivery Options**: Use Delivery Options to forward email messages sent to the user to another recipient and to set the maximum number of recipients that the user can send a message to. Click **Edit details** to view and change these settings.

  - **Forwarding address**: Select the **Enable forwarding** check box and then click **Browse** to display the **Select Mail User and Mailbox** page. Use this page to select a recipient to whom you want to forward all email messages that are sent to this mailbox. Messages will be delivered to both the linked mailbox and the forwarding address.

  - **Recipient limit**: This setting controls the maximum number of recipients the user can send a message to. Select the **Maximum recipients** check box to limit the number of recipients allowed on the To:, Cc:, and Bcc: lines of an email message, and then specify the maximum number of recipients.

    > [!NOTE]
    > For on-premises Exchange organizations, the recipient limit is unlimited. For Exchange Online organizations, the limit is 500 recipients.

- **Message Size Restrictions**: These settings control the size of messages that the user can send and receive. Click **Edit details** to view and change the maximum size for sent and received messages.

  - **Sent messages**: To specify a maximum size for messages sent by this user, select the **Maximum message size (KB)** check box and type a value in the box. The message size must be between 0 and 2,097,151 KB. If the user sends a message larger than the specified size, the message will be returned to the user with a descriptive error message.

  - **Received messages**: To specify a maximum size for messages received by this user, select the **Maximum message size (KB)** check box and type a value in the box. The message size must be between 0 and 2,097,151 KB. If the user receives a message larger than the specified size, the message will be returned to the sender with a descriptive error message.

- **Message Delivery Restrictions**: These settings control who can send email messages to this user. Click **Edit details** to view and change these restrictions.

  - **Accept messages from**: Use this section to specify who can send messages to this user.

    - **All senders**: Select this option to specify that the user can accept messages from all senders. This includes both senders in your Exchange organization and external senders. This option is selected by default. This option includes external users only if you clear the **Require that all senders are authenticated** check box. If you select this check box, messages from external users will be rejected.

    - **Only senders in the following list**: Select this option to specify that the user can accept messages only from a specified set of senders in your Exchange organization. Click **Add** to display the **Select Recipients** page, which displays a list of all recipients in your Exchange organization. Select the recipients you want, add them to the list, and then click **OK**. You can also search for a specific recipient by typing the recipient's name in the search box and then clicking **Search**.

    - **Require that all senders are authenticated**: Select this option to prevent anonymous users from sending messages to the user.

  - **Reject messages from**: Use this section to block people from sending messages to this user.

    - **No senders**: Select this option to specify that the mailbox won't reject messages from any senders in the Exchange organization. This option is selected by default.

    - **Senders in the following list**: Select this option to specify that the mailbox will reject messages from a specified set of senders in your Exchange organization. Click **Add** to display the **Select Recipient** page, which displays a list of all recipients in your Exchange organization. Select the recipients you want to reject messages from, add them to the list, and then click **OK**. You can also search for a specific recipient by typing the recipient's name in the search box and then clicking **Search**.

## Member Of

Use the **Member Of** section to view a list of the distribution groups or security groups to which this user belongs. You can't change membership information on this page. Note that the user may match the criteria for one or more dynamic distribution groups in your organization. However, dynamic distribution groups aren't displayed on this page because their membership is calculated each time they're used.

## MailTip

Use the **MailTip** section to add a MailTip to alert users of potential issues if they send a message to this recipient. A MailTip is text that's displayed in the InfoBar when a recipient is added to the To, Cc, or Bcc lines of a new email message.

> [!NOTE]
> MailTips can include HTML tags, but scripts aren't allowed. The length of a custom MailTip can't exceed 175 displayed characters. HTML tags aren't counted in the limit.

## Mailbox Delegation

Use the **Mailbox Delegation** section to assign permissions to other users (also called *delegates*) to allow them to sign in to the user's mailbox or send messages on behalf of the user. You can assign the following permissions:

- **Send As**: This permission allows users other than the mailbox owner to use the mailbox to send messages. After this permission is assigned to a delegate, any message that a delegate sends from this mailbox will appear as if it was sent by the mailbox owner. However, this permission doesn't allow a delegate to sign in to the user's mailbox.

- **Send on Behalf**: This permission also allows a delegate to use this mailbox to send messages. However, after this permission is assigned to a delegate, the **From:** address in any message sent by the delegate indicates that the message was sent by the delegate on behalf of the mailbox owner.

- **Full Access**: This permission allows a delegate to sign in to the user's mailbox and view the contents of the mailbox. However, after this permission is assigned to a delegate, the delegate can't send messages from the mailbox. To allow a delegate to send email from the user's mailbox, you still have to assign the delegate the Send As or the Send on Behalf Of permission.

To assign permissions to delegates, click **Add** under the appropriate permission to display the **Select Recipient** page, which displays a list of all recipients in your Exchange organization that can be assigned the permission. Select the recipients you want assign delegate permissions to, add them to the list, and then click **OK**. You can also search for a specific recipient by typing the recipient's name in the search box and then clicking **Search**.

## Use the Shell to change linked mailbox properties

Use the **Get-Mailbox** and **Set-Mailbox** cmdlets to view and change properties for linked mailboxes. One advantage of using the Shell is the ability to change the properties for multiple linked mailboxes. For information about what parameters correspond to mailbox properties, see the following topics:

- [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Get-Mailbox)

- [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Set-Mailbox)

Here are some examples of using the Shell to change linked mailbox properties.

This example uses the **Get-Mailbox** command to find all the linked mailboxes in the organization.

```powershell
Get-Mailbox -ResultSize unlimited -Filter "RecipientTypeDetails -eq 'LinkedMailbox'"
```

This example uses the **Set-Mailbox** command to limit the number of recipients allowed on the To:, Cc:, and Bcc: lines of an email message to 500. This limit applies to all linked mailboxes in the organization.

```powershell
Get-Mailbox -ResultSize unlimited -Filter "RecipientTypeDetails -eq 'LinkedMailbox'" | Set-Mailbox -RecipientLimits 500
```

This example changes the linked master account in the fabrikam.com account forest that is associated with a linked mailbox in an Exchange forest.

```powershell
Set-Mailbox -Identity "Ayla Kol" -LinkedDomainController DC1.fabrikam.com -LinkedMasterAccount "fabrikam\robinw" -LinkedCredential:(Get-Credential fabrikam\administrator)
```

## How do you know this worked?

To verify that you have successfully changed properties for a linked mailbox, do the following:

- In the EAC, select the linked mailbox and then click **Edit** to view the property or feature that you changed. Depending on the property that you changed, it might be displayed in the Details pane for the selected mailbox.

- In the Shell, use the **Get-Mailbox** cmdlet to verify the changes. One advantage of using the Shell is that you can view multiple properties for multiple linked mailboxes. In the example above where the recipient limit was changed, running the following command will verify the new value.

  ```powershell
  Get-Mailbox -ResultSize unlimited -Filter "RecipientTypeDetails -eq 'LinkedMailbox'" | fl Name,RecipientLimits
  ```

  For the example above where the linked master account was changed, run the following command to verify the new value.

  ```powershell
  Get-Mailbox "Ayla Kol" | fl LinkedMasterAccount
  ```
