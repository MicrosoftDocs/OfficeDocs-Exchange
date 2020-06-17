---
title: 'Create a sharing policy: Exchange 2013 Help'
TOCTitle: Create a sharing policy
ms:assetid: cae8cab0-6265-448b-8add-5202cdb20678
ms:mtpsurl: https://technet.microsoft.com/library/JJ657494(v=EXCHG.150)
ms:contentKeyID: 49289410
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Create a sharing policy

_**Applies to:** Exchange Server 2013_

You can use sharing policies to control how users in your Exchange organization share calendar information with users outside your organization. Sharing policies provide user-established, people-to-people sharing of calendar information with different types of external users. They support the sharing of calendar information with external federated organizations (such as Office 365 or another on-premises Exchange organization), external non-federated organizations, and individuals with Internet access. To apply a specific sharing policy to users, see [Apply a sharing policy to mailboxes](apply-a-sharing-policy-to-mailboxes-exchange-2013-help.md).

> [!IMPORTANT]
> Creating a sharing policy is one of several steps in setting up federated sharing in your Exchange organization. You have to set up a federation trust with the Azure Active Directory authentication system for your on-premises Exchange organization before you can share calendar information with other federated Exchange organizations. A federation trust isn't required for Internet sharing policies.

To learn more about federated sharing, see [Sharing](sharing-exchange-2013-help.md).

For additional management tasks related to federation, see [Federation procedures](federation-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 15 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Calendar and Sharing Permissions" section in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- The following are required for sharing policies between federated Exchange organizations:

  - An Exchange 2013 Client Access server exists in each Exchange organization. Sharing policies are also supported between Exchange organizations where one organization has Exchange 2013 Client Access servers and the other one organization has Exchange 2010 SP3 or later Client Access servers.

  - Each Exchange organization has created a federation trust with the Azure AD authentication system. For details, see [Configure a federation trust](configure-a-federation-trust-exchange-2013-help.md).

  - Each Exchange organization has configured a federated organization identifier. Domains used for generating users' e-mail addresses have been added to the organization identifiers.

  - User mailboxes are located on Exchange 2013 Mailbox servers or Exchange 2010 Mailbox servers in each Exchange organization.

  - Only Outlook 2010 or later and Outlook Web App users can create sharing invitations.

- The following are required for sharing policies with non-federated Exchange organizations or individuals:

  - An Exchange 2013 Client Access server exists in the Exchange organization that's sharing user's calendar information.

  - User mailboxes are located on Exchange 2013 Mailbox servers in the Exchange organization that's sharing user's calendar information.

  - The Exchange 2013 Client Access server must be enabled for Outlook Web App access.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

## Use the EAC to create a sharing policy

1. Navigate to **organization**\> **sharing**.

2. In the list view, under **Individual Sharing**, click **New** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

3. In **new sharing policy**, type a friendly name for the sharing policy in the **Policy name** box.

4. Click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon") to specify the sharing rules for the sharing policy.

5. In **sharing rule**, select one of the following options to specify the domains you want to share with:

      - **Sharing with all domains**

      - **Sharing with a specific domain**

6. If you select **Sharing with a specific domain**, type the name of the domain you want to share with. If you need to enter more than one domain for this sharing policy, save the settings for the first domain, then edit the sharing rules to add more domains.

7. To define the calendar sharing levels you want to enforce for the policy, select the **Share your calendar folder** check box, and then select one of the following options:

      - **Calendar free/busy information with time only**

      - **Calendar free/busy information with time, subject, and location**

      - **All calendar appointment information, including time, subject, location and title**

8. Click **save** to set the rules for the sharing policy.

9. If you want to make this sharing policy the default sharing policy for users in your Exchange organization, select the **Make this policy my default sharing policy** check box.

10. Click **save** to create the sharing policy.

## Use the EAC to allow all users to share full calendar details

You can edit the default sharing policy to allow all of your users to share full calendar details with people outside of your organization.

1. Navigate to **organization**\> **sharing**.

2. In the list view, under **Individual Sharing**, select **the Default Sharing Policy**, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. In the **Sharing Policy** dialog box, select **Sharing with all domains**, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

4. In the **Sharing Rule** dialog box, under **Specify what information you want to share**, select **All calendar appointment information, including time, subject, location and title**, and then click **save**.

5. In the **Sharing Policy** dialog box, click **save** to set the rules for the sharing policy.

## Use the Shell to create a sharing policy

- This example creates the sharing policy Contoso for the external federated domain contoso.com. This policy allows users in the contoso.com domain to see your user's detailed calendar availability (free/busy) information. By default, this policy is enabled.

  ```powershell
  New-SharingPolicy -Name "Contoso" -Domains contoso.com: CalendarSharingFreeBusyDetail
  ```

- This example creates the sharing policy ContosoWoodgrove for two different federated domains (contoso.com and woodgrovebank.com) with different sharing actions configured for each domain. The policy is disabled.

  ```powershell
  New-SharingPolicy -Name "ContosoWoodgrove" -Domains 'contoso.com: CalendarSharingFreeBusySimple', 'woodgrovebank.com: CalendarSharingFreeBusyDetail -Enabled $false
  ```

- This example creates the sharing policy Anonymous for an Exchange organization with the Client Access server CAS01 and the Mailbox server MAIL01 with the sharing action configured for limited calendar availability information. This policy allows users in your Exchange organization to invite users with Internet access to view their calendar availability information by sending them a link. The policy is enabled.

  1. Set the Web proxy URL for MAIL01.

     ```powershell
     Set-ExchangeServer -Identity "Mail01" -InternetWebProxy "<Webproxy URL>"
     ```

  2. Enable the publishing virtual directory on CAS01.

     ```powershell
     Set-OwaVirtualDirectory -Identity "CAS01" -ExternalURL "<URL for CAS01>" -CalendarPublishingEnabled $true
     ```

  3. Create the sharing policy Anonymous and configure limited calendar information sharing.

     ```powershell
     New-SharingPolicy -Name "Anonymous" -Domains 'Anonymous: CalendarSharingFreeBusySimple' -Enabled $true
     ```

For detailed syntax and parameter information, see the following topics:

- [New-SharingPolicy](https://docs.microsoft.com/powershell/module/exchange/New-SharingPolicy)

- [Set-ExchangeServer](https://docs.microsoft.com/powershell/module/exchange/Set-ExchangeServer)

- [Set-OwaVirtualDirectory](https://docs.microsoft.com/powershell/module/exchange/Set-OwaVirtualDirectory)

## How do you know this worked?

To verify that you have successfully created the sharing policy, run the following Shell command to verify the sharing policy information.

```powershell
Get-SharingPolicy <policy name> | format-list
```

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).
