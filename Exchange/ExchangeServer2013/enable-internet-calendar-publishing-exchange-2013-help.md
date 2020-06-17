---
title: 'Enable Internet calendar publishing: Exchange 2013 Help'
TOCTitle: Enable Internet calendar publishing
ms:assetid: b4c71696-52bb-492c-8259-0e419acd0bbc
ms:mtpsurl: https://technet.microsoft.com/library/JJ853046(v=EXCHG.150)
ms:contentKeyID: 50390855
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Enable Internet calendar publishing

_**Applies to:** Exchange Server 2013_

**Summary:** Use these procedures to enable OWA users in your Exchange 2013 organization to share calendar free/busy information with external organizations.

Users in Microsoft Exchange Server 2013 organizations can share calendar availability (free/busy) information with users in non-Exchange organizations and other individuals with Internet access. Internet calendar publishing provides increased flexibility and increases the number of users who can share calendar availability information.

Enabling Internet calendar publishing consists of three general steps:

1. Configure the Web proxy URL for the Mailbox server (this step is only necessary if a Web proxy URL already exists in your organization, otherwise skip to step 2).

2. Enable the publishing virtual directory for the Client Access server.

3. Create a dedicated sharing policy specifically for Internet calendar publishing or update the default sharing policy to support the **Anonymous** domain. Either method allows users in your Exchange organization to invite other users who have Internet access to view limited calendar availability information by accessing a published URL.

> [!IMPORTANT]
> Upon completing Step 3, users will then need to publish their calendars from Outlook. Calendar publishing creates URLs that users can give to people outside their organization. One URL lets recipients subscribe to calendars by using Outlook or Outlook Web App, and the other lets the recipient view a calendar in a Web browser. Each user can control how much detail others can see.

For additional management tasks related to sharing policies, see [Sharing policies](sharing-policies-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete this task: 15 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Calendar and Sharing Permissions" entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- An Exchange 2013 Client Access server exists in the Exchange organization that's sharing users' calendar information.

- User mailboxes are on Exchange 2013 Mailbox servers in the Exchange organization that's sharing users' calendar information.

- Only Outlook 2010 or later and Outlook Web App users can create sharing invitations.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Step 1: Use the Shell to configure the Web proxy URL

> [!NOTE]
> This step is only necessary if a Web proxy URL already exists in your organization. If not, skip to Step 2.<BR>You can't use the Exchange admin center (EAC) to configure the Web proxy URL.

This example configures a Web proxy URL on Mailbox server MAIL01.

```powershell
Set-ExchangeServer -Identity "MAIL01" -InternetWebProxy "<Webproxy URL>"
```

For detailed syntax and parameter information, see [Set-ExchangeServer](https://docs.microsoft.com/powershell/module/exchange/Set-ExchangeServer).

### How do you know this step worked?

To verify that you have successfully configured the Web proxy URL, run the following Shell command and verify the *InternetWebProxy* parameter information.

```powershell
Get-ExchangeServer | format-list
```

## Step 2: Use the Shell to enable the publishing virtual directory

> [!NOTE]
> You can't use the EAC to enable the publishing virtual directory.

This example enables the publishing virtual directory on Client Access server CAS01.

```powershell
Set-OwaVirtualDirectory -Identity "CAS01\owa (Default Web Site)" -ExternalUrl "<URL for CAS01>" -CalendarEnabled $true
```

Where the identity `CAS01\owa (Default Web Site)` is both the server name and the Outlook Web App virtual directory.

For detailed syntax and parameter information, see [Set-OwaVirtualDirectory](https://docs.microsoft.com/powershell/module/exchange/Set-OwaVirtualDirectory).

### How do you know this step worked?

To verify that you have successfully enabled the publishing virtual directory, run the following Shell command and verify the *ExternalURL* parameter information.

```powershell
Get-OwaVirtualDirectory | format-list
```

## Step 3: Create or configure a sharing policy specifically for Internet calendar publishing

> [!NOTE]
> The following options in Step 3 only apply to Exchange Online environments.

You have the choice of creating a sharing policy for Internet calendar publishing (option 1) or configuring the default sharing policy for Internet calendar publishing (option 2). With both options you have the choice of using the EAC or the Shell.

### Option 1: Create a sharing policy specifically for Internet calendar publishing

If you want to create a sharing policy specifically for Internet calendar publishing, complete the following steps.

#### Use the EAC

1. Navigate to **Organization**\> **Sharing**.

2. In the list view, under **Individual Sharing**, click **New** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

3. In **Sharing Policy**, type a friendly name for the sharing policy in the **Policy name** field (for example, **Internet**).

4. Click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon") to define the sharing rules for the sharing policy.

5. In **Sharing Rule**, click **Sharing with a specific domain**, and then type **Anonymous** in the corresponding box.

6. To specify the calendar sharing levels you want to enforce for the sharing policy, select the **Share your calendar folder** check box, and then select one of the following:

   - **Calendar free/busy information with time only**

   - **Calendar free/busy information with time, subject, and location**

   - **All calendar appointment information, including time, subject, location and title**

7. Click **Save** to set the rules for the sharing policy.

8. In **Sharing Policy**, click **Save** to create the policy.

#### Use the Shell

This example creates an Internet calendar publishing sharing policy named Internet and configures the policy to share only availability information. The policy is enabled.

```powershell
New-SharingPolicy -Name "Internet" -Domains 'Anonymous: CalendarSharingFreeBusySimple' -Enabled $true
```

This example adds the sharing policy Internet to a user mailbox.

```powershell
Set-Mailbox -Identity <user name> -SharingPolicy "Internet"
```

This example adds the sharing policy Internet to an organizational unit (OU).

```powershell
Set-Mailbox -OrganizationalUnit <OU name> -SharingPolicy "Internet"
```

For detailed syntax and parameter information, see [New-SharingPolicy](https://docs.microsoft.com/powershell/module/exchange/New-SharingPolicy) and [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Set-Mailbox).

### How do you know this step worked?

To verify that you have successfully created the sharing policy, run the following Shell command to verify the sharing policy information.

```powershell
Get-SharingPolicy <policy name> | format-list
```

### Option 2: Configure the default sharing policy for Internet calendar publishing

If you want to configure the default sharing policy for Internet calendar publishing, complete the following steps.

#### Use the EAC

1. Navigate to **Organization** \> **Sharing**.

2. In the list view, under **Individual Sharing**, select the Default Sharing Policy, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. In **Sharing Policy**, click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon") to add a sharing rule to the policy.

4. In **Sharing Rule**, click **Sharing with a specific domain**, and then and then type **Anonymous** in the corresponding box.

5. To specify the calendar sharing levels you want to enforce for the sharing policy, select the **Share your calendar folder** check box, and then select one of the following:

   - **Calendar free/busy information with time only**

   - **Calendar free/busy information with time, subject, and location**

   - **All calendar appointment information, including time, subject, location and title**

6. Click **Save** to set the rules for the sharing policy.

7. In **Sharing Policy**, click **Save** to save the changes.

#### Use the Shell

This example updates the Default Sharing Policy and configures the policy to share only availability information. The policy is enabled.

```powershell
Set-SharingPolicy -Name "Default Sharing Policy" -Domains 'Anonymous: CalendarSharingFreeBusySimple' -Enabled $true
```

For detailed syntax and parameter information, see [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Set-Mailbox).

### How do you know this step worked?

To verify that you have successfully updated the Default Sharing Policy, run the following Shell command to verify the sharing policy information.

```powershell
Get-SharingPolicy <policy name> | format-list
```
