---
localization_priority: Normal
description: Admins can learn how to add, modify, and remove remote domains (message formatting settings for external domains) in Exchange Online.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: d3dca7b0-c84c-429a-9698-0e92a95a0985
ms.reviewer: 
title: Manage remote domains in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
f1.keywords:
- NOCSH
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Manage remote domains in Exchange Online

Remote domains define settings based on the destination domain of each email message. All organizations have a default remote domain named "Default" that's applied to the domain "*". The default remote domain applies the same settings to all email messages regardless of the destination domain. However, you can configure specific settings for a specific destination domain.

The following table shows the default values for common settings:

|**Setting**|**Default**|
|:-----|:-----|
|Out of office replies|Send external out of office replies to people on the remote domain.|
|Automatic replies|Allow automatic replies or automatically forwarded messages to be sent to people on the remote domain.|
|Delivery and non-delivery reports|Allow delivery and non-delivery reports to be sent to people on the remote domain.|
|Meeting forward notifications|Don't allow meeting forward notifications to be sent to people on the remote domain.|
|Rich Text format (RTF)|Follow settings created by each user in Outlook or Outlook on the web (formerly known as Outlook Web App) when a message is sent to people on the remote domain.|
|Supported character set|Do not specify a MIME or non-MIME character set if the character set isn't specified in the message sent to the remote domain.|

For information about when to configure remote domains, descriptions of the available settings, and information about how remote domain settings override per-user settings, see [Remote domains in Exchange Online](remote-domains.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mail flow" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- To open the Exchange admin center (EAC), see [Exchange admin center in Exchange Online](../../exchange-admin-center.md). To connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Create and configure remote domains

**Notes**:

- If you create a remote domain for a specific destination domain, and a setting for the specific remote domain conflicts with the same setting in the default remote domain, the setting for the specific remote domain overrides the setting in the default remote domain.

- Once you've created a remote domain, you can't change or replace the domain inside the remote domain. Instead, create and configure a new remote domain with the new domain name.

### Use the EAC to create and configure a remote domain

1. In the EAC, go to **Mail flow** \> **Remote domains**.

2. To create a new domain:

   1. Select **New** ![Add Icon](../../media/ITPro_EAC_AddIcon.png).

   2. In the **Name** box, enter a descriptive name for the domain.

   3. In the **Remote Domain** box, enter the full domain name. Use the wildcard character (\*) for all subdomains of a specified domain, for example, \*.contoso.com.

3. To change settings for the default domain, select **Default**, and then select **Edit**.

4. Select the options you want:

   - In the **Out of Office reply types** section, specify which type of out of office replies should be sent to people at this domain.

   - In the **Automatic replies** section, specify whether you want to allow automatic replies, automatic forwarding, or both.

   - In the **Message reporting** section, specify:

   - Whether you want to allow delivery reports and non-delivery reports.

   - If a meeting set up by someone on the remote domain is forwarded to another person in your organization, whether the notification message should go to the meeting organizer on the remote domain.

   - In the **Use Rich-text format** section, specify whether to follow each user's message settings, or whether to always or never preserve RTF formatting. Selecting **Never** means that RTF messages are sent as plain text or HTML.

   - In the **Supported Character Set** area, specify which character set to use if the message doesn't specify the character set.

5. Click **Save**. If you created a new remote domain, it is added to the list.

### Use Exchange Online PowerShell to create and configure a remote domain

After you create the remote domain, you can configure the settings (you can't create the remote domain and configure the settings in one step).

#### Step 1: Create the remote domain

To create a new remote domain, use the following syntax:

```powershell
New-RemoteDomain -Name "<Unique Name"> -DomainName <single SMTP domain | domain with subdomains>
```

This example creates a remote domain for messages sent to the contoso.com domain.

```powershell
New-RemoteDomain -Name Contoso -DomainName contoso.com
```

This example creates a remote domain for messages sent to the contoso.com domain and all its subdomains.

```powershell
New-RemoteDomain -Name "Contoso and subdomains" -DomainName *.contoso.com
```

For detailed syntax and parameter information, see [New-RemoteDomain](https://docs.microsoft.com/powershell/module/exchange/new-remotedomain).

#### Step 2: Configure the remote domain settings

To configure the settings for a remote domain, use the following syntax:

```powershell
Set-RemoteDomain -Identity <Name> [-AllowedOOfType <External | InternalLegacy | ExternalLegacy | None>] [-AutoForwardEnabled <$true | $false>] [-AutoReplyEnabled <$true | $false>] [-CharacterSet <SupportedCharacterSet>] [-DeliveryReportEnabled <$true | $false>] [-NonMimeCharacterSet <SupportedCharacterSet>] [-TNEFEnabled <$true | $false>]
```

This example disables automatic replies, automatic forwarding, and out-of-office replies to recipients at all remote domains that aren't specified with their own remote domain.

```powershell
Set-RemoteDomain -Identity  Default -AutoReplyEnabled $false -AutoForwardEnabled $false -AllowedOOFType None
```

This example sends internal out of office replies to users at the remote domain named Contoso.

```powershell
Set-RemoteDomain -Identity Contoso -AllowedOOFType InternalLegacy
```

This example disables prevents delivery reports and non-delivery reports from being sent to users at Contoso.

```powershell
Set-RemoteDomain -Identity Contoso -DeliveryReportEnabled $false -NDREnabled $false
```

This example sends all messages to Contoso using Transport Neutral Encapsulation Formation (TNEF) encoding, rather than MIME encoding. This preserves Rich Text format in messages.

```powershell
Set-RemoteDomain -Identity Contoso -TNEFEnabled $true
```

This example sends all messages to Contoso using MIME encoding, which means that all RTF messages are always converted to HTML or plain text.

```powershell
Set-RemoteDomain -Identity Contoso -TNEFEnabled $false
```

This example uses the message format settings the user has defined in Outlook or Outlook on the web for encoding messages.

```powershell
Set-RemoteDomain -Identity Contoso -TNEFEnabled $null
```

This example uses the Korean (ISO) character set for MIME messages sent to Contoso.

```powershell
Set-RemoteDomain -Identity Contoso -CharacterSet iso-2022-kr
```

This example specifies using the Unicode character set for non-MIME messages sent to Contoso.

```powershell
Set-RemoteDomain -Identity Contoso -NonMimeCharacterSet utf-8
```

For detailed syntax and parameter information, see [Set-RemoteDomain](https://docs.microsoft.com/powershell/module/exchange/set-remotedomain).

### How do you know this worked?

To verify that you've successfully created and configured a remote domain, use either of the following steps:

- In the EAC, go to **Mail flow** \> **Remote domains**, select the remote domain, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png) to verify the settings.

- In Exchange Online PowerShell, replace \<Remote Domain Name\> with the name of the remote domain and run the following command to verify the settings:

  ```powershell
  Get-RemoteDomain -Identity "<Remote Domain Name>" | Format-List
  ```

## Remove remote domains

**Notes:**

- You can't remove the default remote domain.

- When you remove a remote domain, the default remote domain settings will then apply to messages sent to that domain.

- Removing a remote domain doesn't disable mail flow to the remote domain.

### Use the EAC to remove a remote domain

1. In the EAC, go to **Mail flow** \> **Remote domains**.

2. Select a remote domain, and then select **Delete** ![Delete icon](../../media/ITPro_EAC_DeleteIcon.png).

3. In the warning dialog box, select **Yes**.

### Use Exchange Online PowerShell to remove a remote domain

To remove a remote domain, use the following syntax:

```powershell
Remove-RemoteDomain -Identity <Remote Domain Name>
```

This example removes the remote domain named Contoso.

```powershell
Remove-RemoteDomain -Identity Contoso
```

For detailed syntax and parameter information, see [Remove-RemoteDomain](https://docs.microsoft.com/powershell/module/exchange/remove-remotedomain).

### How do you know this worked?

To verify that you've successfully removed a remote domain, do either of the following steps:

- In the EAC, go to **Mail flow** \> **Remote domains** and verify the remote domain isn't listed.

- In Exchange Online PowerShell, run the following command and verify that the remote domain isn't listed:

  ```powershell
  Get-RemoteDomain
  ```
