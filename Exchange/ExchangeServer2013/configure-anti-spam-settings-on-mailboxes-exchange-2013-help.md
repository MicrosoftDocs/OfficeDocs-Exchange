---
title: 'Configure Anti-Spam Settings on Mailboxes: Exchange 2013 Help'
TOCTitle: Configure Anti-Spam Settings on Mailboxes
ms:assetid: 868d7fd8-e817-46ba-9b67-edf2f50b9494
ms:mtpsurl: https://technet.microsoft.com/en-us/library/Bb123559(v=EXCHG.150)
ms:contentKeyID: 49345051
ms.date: 12/09/2016
ms.reviewer: 
manager: dansimp
ms.author: dmaguire
author: msdmaguire
mtps_version: v=EXCHG.150
---

# Configure Anti-Spam Settings on Mailboxes

_**Applies to:** Exchange Server 2013_

You can configure specific anti-spam settings on individual mailboxes that are different than the anti-spam settings that are applied to the rest of the mailboxes in your Exchange organization. When you configure an anti-spam setting on a mailbox, that setting overrides the corresponding organization-wide content filtering or organization configuration anti-spam setting.

> [!NOTE]
> On November 1, 2016, Microsoft stopped producing spam definition updates for the SmartScreen filters in Exchange and Outlook. The existing SmartScreen spam definitions will be left in place, but their effectiveness will likely degrade over time. For more information, see <A href="https://go.microsoft.com/fwlink/p/?linkid=835894">Deprecating support for SmartScreen in Outlook and Exchange</A>.

## What do you need to know before you begin?

- Estimated time to complete each procedure: 15 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Anti-spam features" entry in the [Anti-spam and anti-malware permissions](anti-spam-and-anti-malware-permissions-exchange-2013-help.md) topic, and the "Anti-spam" entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- By default, anti-spam features aren't enabled in the Transport service on a Mailbox server. Typically, you only enable the anti-spam features on a Mailbox server if your Exchange organization doesn't do any prior anti-spam filtering before accepting incoming messages. For more information, see [Enable anti-spam functionality on Mailbox servers](enable-anti-spam-functionality-on-mailbox-servers-exchange-2013-help.md).

- You can only use the Shell to perform this procedure.

- The Junk Email Folder SCL threshold value behaves differently than the SCL delete, reject, and quarantine values. For more information, see [Spam Confidence Level Threshold](spam-confidence-level-threshold-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to configure anti-spam features on a single mailbox

To configure the anti-spam settings on a single mailbox, use the following syntax.

```powershell
Set-Mailbox <MailboxIdentity> -AntispamBypassEnabled <$true | $false> -RequireSenderAuthenticationEnabled <$true | $false> -SCLDeleteEnabled <$true | $false | $null> -SCLDeleteThreshold <0-9 | $null> -SCLJunkEnabled <$true | $false | $null > -SCLJunkThreshold <0-9 | $null> -SCLQuarantineEnabled <$true | $false | $null > -SCLQuarantineThreshold <0-9 | $null> -SCLRejectEnabled <$true | $false | $null > -SCLRejectThreshold <0-9 | $null>
```

This example configures the mailbox of a user named Jeff Phillips to bypass all the anti-spam filters and to have messages that meet or exceed a Junk Email folder SCL threshold of 5 delivered to his Junk Email folder in Microsoft Outlook.

```powershell
Set-Mailbox "Jeff Phillips" -AntispamBypassEnabled $true -SCLJunkEnabled $true -SCLJunkThreshold 4
```

## How do you know this worked?

To verify that you have successfully configured the anti-spam features on a single mailbox, do the following:

1. Run the following command:

   ```powershell
   Get-Mailbox <MailboxIdentity> | Format-List SCL*,Bypass*,*SenderAuth*
   ```

2. Verify the value displayed is the value you configured.

## Use the Shell to configure anti-spam features on multiple mailboxes

To configure all the anti-spam settings on multiple mailboxes, use the following syntax.

```powershell
Get-Mailbox [<Filter>]| Set-Mailbox <Anti-Spam Settings>
```

This example enables the SCL quarantine threshold with a value of 7 on all mailboxes in the Users container in the Contoso.com domain.

```powershell
Get-Mailbox -OrganizationalUnit Contoso.com/Users | Set-Mailbox -SCLQuarantineEnabled $true -SCLQuarantineThreshold 7
```

## How do you know this worked?

To verify that you have successfully configured the anti-spam features on multiple mailboxes, do the following:

1. Run the following command:

   ```powershell
   Get-Mailbox [<Filter>] | Format-List Name,SCL*,*SenderAuth*
   ```

2. Verify the values displayed are the values you configured.

## Use the Shell to configure the junk email threshold for all mailboxes in your organization

Run the following command:

```powershell
Set-OrganizationConfig -SCLJunkThreshold <Integer>
```

This example sets the organization's junk email threshold to 5.

```powershell
Set-OrganizationConfig -SCLJunkThreshold 5
```

## How do you know this worked?

To verify that you have successfully configured the junk email threshold for all mailboxes in your organization, do the following:

1. Run the following command:

   ```powershell
   Get-OrganizationConfig | Format-List SCLJunkThreshold
   ```

2. Verify the value displayed is the value you configured.
