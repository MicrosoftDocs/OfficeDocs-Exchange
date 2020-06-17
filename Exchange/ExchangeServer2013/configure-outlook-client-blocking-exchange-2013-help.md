---
title: 'Configure Outlook client blocking: Exchange 2013 Help'
TOCTitle: Configure Outlook client blocking
ms:assetid: 3a579c83-8bc7-4adc-a25c-8eb6eed7220c
ms:mtpsurl: https://technet.microsoft.com/library/Dd335207(v=EXCHG.150)
ms:contentKeyID: 50873794
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure Outlook client blocking

_**Applies to:** Exchange Server 2013_

In Exchange Server 2013, you can use retention policies or managed folders for messaging records management (MRM). Only users running Microsoft Outlook 2010 and later have access to all client features for retention policies. However, retention policies are applied on the Mailbox server by the Managed Folder Assistant, regardless of the Outlook client version used by the user. Older Outlook clients do not expose the MRM functionality of these features. For example, because Outlook 2007 does not support retention policies, users can't apply personal tags to items or folders.

You can block users who are running older versions of Outlook from accessing their Exchange mailboxes. You can also block access on a per-mailbox or on a per-Client Access server basis.

For other management tasks related to MRM, see [Messaging Records Management Procedures](https://docs.microsoft.com/office365/securitycompliance/inactive-mailboxes-in-office-365).

## MRM Feature Availability by Client Application and Version

The following table lists the MRM features available in various client applications and versions.

### MRM features

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Client application</th>
<th>Available MRM client features</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Outlook 2013 and Outlook 2010</p></td>
<td><p>All</p></td>
</tr>
<tr class="even">
<td><p>Outlook 2007</p></td>
<td><p>Managed folders</p></td>
</tr>
<tr class="odd">
<td><p>Outlook 2003 and older</p></td>
<td><p>Not supported</p></td>
</tr>
<tr class="even">
<td><p>Other e-mail client software</p></td>
<td><p>None</p></td>
</tr>
</tbody>
</table>

The following table shows version numbers for Outlook.

### Outlook versions

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Outlook version</th>
<th>Version number</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Outlook 2013</p></td>
<td><p>15</p></td>
</tr>
<tr class="even">
<td><p>Outlook 2010</p></td>
<td><p>14</p></td>
</tr>
<tr class="odd">
<td><p>Outlook 2007</p></td>
<td><p>12</p></td>
</tr>
<tr class="even">
<td><p>Outlook 2003</p></td>
<td><p>11</p></td>
</tr>
<tr class="odd">
<td><p>Outlook 2002</p></td>
<td><p>10</p></td>
</tr>
<tr class="even">
<td><p>Outlook 2000</p></td>
<td><p>9</p></td>
</tr>
<tr class="odd">
<td><p>Outlook 98</p></td>
<td><p>8.5</p></td>
</tr>
<tr class="even">
<td><p>Outlook 97</p></td>
<td><p>8</p></td>
</tr>
</tbody>
</table>

> [!NOTE]
> Before making any changes, note that hotfixes and service pack releases may affect the client version string. Be careful when you restrict client access because server-side Exchange components must also use MAPI to log on. Some components report their client version as the component name (such as SMTP or OLE&nbsp;DB), while others report the Exchange build number (such as 6.0.4712.0). For this reason, avoid restricting clients that have version numbers that start with 6.&lt;<EM>x</EM>.<EM>x</EM>.&gt;. For example, to prevent MAPI access completely, instead of specifying <STRONG>0.0.0-6.5535.65535.65535</STRONG>, specify the two ranges so that the server components can log on. For example, specify the following: <STRONG>0.0.0-5.9.9; 7.0.0-</STRONG>.

After you perform these procedures, be aware that when users are blocked from accessing their mailboxes, they will receive the following warning message.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>Your Exchange Server administrator has blocked the version of Outlook that you are using. Contact your administrator for assistance.</p></td>
</tr>
</tbody>
</table>

To bypass the warning that MRM features aren't supported for e-mail clients running versions of Outlook earlier than Outlook 2010, you can use the *ManagedFolderMailboxPolicyAllowed* parameter of the **New-Mailbox**, **Enable-Mailbox**, and **Set-Mailbox** cmdlets in the Shell. When a managed folder mailbox policy is assigned to a mailbox by using the *ManagedFolderMailboxPolicy* parameter, the warning appears by default unless you use the *ManagedFolderMailboxPolicyAllowed* parameter.

## What do you need to know before you begin?

- Estimated time to complete: 1 minute.

- You can't use the Exchange admin center (EAC) to perform these procedures. You must use the Shell.

- Procedures in this topic require specific permissions. See each procedure for its permissions information.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Block versions of Outlook on a per-mailbox basis

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "User mailboxes" entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

This example blocks all Outlook versions earlier than 11.8010.8036.

```powershell
Set-CASMailbox -Identity adam@contoso.com -MAPIBlockOutlookVersions "-11.8010.8036"
```

This example restores access to a mailbox that's blocked by a version of Outlook.

```powershell
Set-CASMailbox -Identity adam@contoso.com -MAPIBlockOutlookVersions $null
```

For detailed syntax and parameter information, see [Set-CASMailbox](https://docs.microsoft.com/powershell/module/exchange/Set-CASMailbox).

## Block Outlook versions on a Client Access server

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "RPC Client Access settings" entry in the [Clients and mobile devices permissions](clients-and-mobile-devices-permissions-exchange-2013-help.md) topic.

This example blocks Outlook clients prior to version 12.0.0 from accessing the mailbox on an Exchange 2010 or later Client Access server.

> [!IMPORTANT]
> The values used with the <EM>BlockedClientVersions</EM> parameter are examples. You can determine the correct client software versions by parsing the RPC Client Access log files located at <CODE>%ExchangeInstallPath%Logging\RPC Client Access</CODE>.

```powershell
Set-RpcClientAccess -Server CAS01 -BlockedClientVersions "0.0.0-5.65535.65535;7.0.0;8.02.4-11.65535.65535"
```

For detailed syntax and parameter definition, see [Set-RpcClientAccess](https://docs.microsoft.com/powershell/module/exchange/Set-RpcClientAccess).
