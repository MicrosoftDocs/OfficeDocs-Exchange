---
title: 'Managed Store Limits: Exchange 2013 Help'
TOCTitle: Managed Store Limits
ms:assetid: bea9ec15-bfb5-4716-b14e-010e389c9f9e
ms:mtpsurl: https://technet.microsoft.com/library/Mt741981(v=EXCHG.150)
ms:contentKeyID: 73225999
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Managed Store Limits in Exchange 2013

**Summary:** Administrators can learn about Managed Store connection limits in Exchange Server 2013 and how to configure them.

In Microsoft Exchange Server 2013, connection and usage limits have been placed on the Exchange Managed Store to prevent a single application or a single user from using all the available connections to the Managed Store. If a single user or application is allowed to use all of the connections, other users or applications cannot be able to access the Managed Store, which could result in downtime.

> [!NOTE]
> For any connections made by accounts that have administrative privileges, the maximum session limits have been increased to 64,000.

## Terminology

Knowledge of the following terms will help you understand the types of connections referenced in this topic.

- **Sessions**

  Sessions represent the connections used by services and client applications, such as Microsoft Outlook, to connect to the Managed Store. Services and clients can have multiple sessions at a particular time. The terms *connections* and *sessions* can be used interchangeably.

- **Threads**

  Threads represent concurrently executing requests to the Managed Store. For example, if a user opens a folder in Outlook, Outlook executes a request to the Managed Store on behalf of the user. That execution of the request is a single thread.

  In Exchange Server 2013, there are no longer thread limits based on client type. Instead, for all clients, the maximum number of threads **per mailbox database** is 50. The exception is the Availability service, which has a maximum limit of 16 per user.

## Session Limits

The following table lists the types of client connections to the Managed Store and the limits based on those connections. If you want to modify the session limits, see "Configure Session Limits" immediately following the table.

Previous versions of Exchange set limits on the number of connections to the Managed Store based on the number of connections per server. In Exchange 2013, session limits are based on connections per mailbox database.

The types of connection limits in Exchange 2013 are as follows:

- **Max sessions per process**: Specifies the maximum number of sessions that an Exchange service can have open at one time on a mailbox database.

- **Max user sessions per process**: Specifies the maximum number of sessions for a particular protocol for a single user.

The following section, "Configure Session Limits," describes how to modify these limits.

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Client type</th>
<th></th>
<th>Max sessions per mailbox database</th>
<th>Default number of user sessions per mailbox database</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Admin</p></td>
<td></td>
<td><p>10,000</p></td>
<td><p>Not applicable</p></td>
</tr>
<tr class="even">
<td><p>Availability service</p></td>
<td></td>
<td><p>10,000</p></td>
<td><p>16</p></td>
</tr>
<tr class="odd">
<td><p>Content indexing</p></td>
<td></td>
<td><p>10,000</p></td>
<td><p>Not applicable</p></td>
</tr>
<tr class="even">
<td><p>Exchange ActiveSync</p></td>
<td></td>
<td><p>Not applicable</p></td>
<td><p>16</p></td>
</tr>
<tr class="odd">
<td><p>Exchange Web Services</p></td>
<td></td>
<td><p>Not applicable</p></td>
<td><p>16</p></td>
</tr>
<tr class="even">
<td><p>Management</p></td>
<td></td>
<td><p>Not applicable</p></td>
<td><p>16</p></td>
</tr>
<tr class="odd">
<td><p>MAPI on the Middle Tier (MoMT)</p></td>
<td></td>
<td><p>Not applicable</p></td>
<td><p>32</p></td>
</tr>
<tr class="even">
<td><p>MSExchangeMailboxAssistants: Events</p></td>
<td></td>
<td><p>10,000</p></td>
<td><p>Not applicable</p></td>
</tr>
<tr class="odd">
<td><p>MSExchangeMailboxAssistants: Timed</p></td>
<td></td>
<td><p>10,000</p></td>
<td><p>Not applicable</p></td>
</tr>
<tr class="even">
<td><p>MSExchange Remote Procedure Call</p></td>
<td></td>
<td><p>Not applicable</p></td>
<td><p>16</p></td>
</tr>
<tr class="odd">
<td><p>Microsoft Office Outlook Web App</p></td>
<td></td>
<td><p>Not applicable</p></td>
<td><p>16</p></td>
</tr>
<tr class="even">
<td><p>POP3 and IMAP4</p></td>
<td></td>
<td><p>Not applicable</p></td>
<td><p>16</p></td>
</tr>
<tr class="odd">
<td><p>Transport</p></td>
<td></td>
<td><p>10,000</p></td>
<td><p>Not applicable</p></td>
</tr>
<tr class="even">
<td><p>Unified Messaging</p></td>
<td></td>
<td><p>Not applicable</p></td>
<td><p>16</p></td>
</tr>
<tr class="odd">
<td><p>Others</p></td>
<td></td>
<td><p>Not applicable</p></td>
<td><p>16</p></td>
</tr>
</tbody>
</table>

## Configure Session Limits

You can modify the default session limits.

> [!NOTE]
> If you want to modify the session limits, you need to modify them on all Mailbox servers within any database availability groups (DAGs). If you don't make the same changes on all servers, the results will be inconsistent. To increase the session limit on Client Access server, the <CODE>RCAMaxConcurrency</CODE> value must be increased on the throttling policy. For more information, see <A href="https://docs.microsoft.com/powershell/module/exchange/Set-ThrottlingPolicy">Set-ThrottlingPolicy</A>.

> [!WARNING]
> Incorrectly editing the registry can cause serious problems that may require you to reinstall your operating system. Problems resulting from editing the registry incorrectly may not be able to be resolved. Before editing the registry, back up any valuable data.

1. Start Registry Editor (regedit).

2. Navigate to the following registry subkey:

    **\\\\HKEY\_LOCAL\_MACHINE \\SYSTEM\\CurrentControlSet\\Services\\MSExchangeIS\\ParametersSystem**.

3. Right-click **ParametersSystem**, point to **New**, and then click **DWORD (32-bit) Value.**

    The new value is created in the result pane.

4. Rename the key to one of the following values, and then press Enter:

      - **Maximum Allowed Sessions Per User**: This limit specifies the maximum allowable sessions per user.

      - **Maximum Allowed Service Sessions Per User**: This limit specifies the maximum allowed service sessions per user.

      - **Maximum Allowed Exchange Sessions Per Service**: This limit specifies the maximum allowed Exchange sessions per service. The default value is 10,000.

5. Right-click the newly created key, and then click **Modify**.

6. In the **Value** **data** box, type the number of objects to which you want to limit this entry, and then click **OK**. Use the preceding table to view the default settings.

## Open Item Limits

Open item limits are limits placed on the number of items that can be opened by a single mailbox in a single session. However, a user can have multiple sessions opened simultaneously. For example, if a user has two sessions opened, the user could open 1,000 folders.

If you want to modify these limits, see "Configure Open Item Limits" immediately following the table.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Item type</th>
<th>Registry object type</th>
<th>Max opened per session</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>ACL View</p></td>
<td><p>objtACLView</p></td>
<td><p>500</p></td>
</tr>
<tr class="even">
<td><p>Attachment</p></td>
<td><p>objtAttachment</p></td>
<td><p>500</p></td>
</tr>
<tr class="odd">
<td><p>Attachment View</p></td>
<td><p>objtAttachmentView</p></td>
<td><p>500</p></td>
</tr>
<tr class="even">
<td><p>CStream</p></td>
<td><p>objtCStream</p></td>
<td><p>Not applicable</p></td>
</tr>
<tr class="odd">
<td><p>Folder</p></td>
<td><p>objtFolder</p></td>
<td><p>500</p></td>
</tr>
<tr class="even">
<td><p>Folder View</p></td>
<td><p>objtFolderView</p></td>
<td><p>500</p></td>
</tr>
<tr class="odd">
<td><p>FX Destination Stream</p></td>
<td><p>objtFXDstStrm</p></td>
<td><p>500</p></td>
</tr>
<tr class="even">
<td><p>FX Source Stream</p></td>
<td><p>objtFXSrcStrm</p></td>
<td><p>500</p></td>
</tr>
<tr class="odd">
<td><p>Message</p></td>
<td><p>objtMessage</p></td>
<td><p>250</p></td>
</tr>
<tr class="even">
<td><p>Message View</p></td>
<td><p>objtMessageView</p></td>
<td><p>500</p></td>
</tr>
<tr class="odd">
<td><p>Notification</p></td>
<td><p>objtNotify</p></td>
<td><p>500,000</p></td>
</tr>
<tr class="even">
<td><p>Rule View</p></td>
<td><p>objtRulesView</p></td>
<td><p>Not applicable</p></td>
</tr>
<tr class="odd">
<td><p>Stream</p></td>
<td><p>objtStream</p></td>
<td><p>250</p></td>
</tr>
</tbody>
</table>

## Configure Open Item Limits

You can limit the maximum number of resources that a MAPI client can use simultaneously.

> [!NOTE]
> If you want to modify the open item limits, you need to modify them on all Mailbox servers within any DAGs and client access arrays. If you don't make the same changes on all servers, the results will be inconsistent.

> [!WARNING]
> Incorrectly editing the registry can cause serious problems that may require you to reinstall your operating system. Problems resulting from editing the registry incorrectly may not be able to be resolved. Before editing the registry, back up any valuable data.

1. Start Registry Editor (regedit).

2. Navigate to the following registry subkey:

    **\\\\HKEY\_LOCAL\_MACHINE \\SYSTEM\\CurrentControlSet\\Services\\MSExchangeIS\\ParametersSystem**

3. Right-click **ParametersSystem**, point to **New**, and then click **Key**.

    A new key is created in the console tree.

4. Rename the key **MaxObjsPerMapiSession**, and then press Enter.

5. Right-click **MaxObjsPerMapiSession**, point to **New**, and then click **DWORD (32-bit) Value**.

    The new value is created in the result pane.

6. Rename the key to *\<Object\_type\>*, where *\<Object\_type\>* is the name of the registry object type that you're modifying. For example, to modify the number of messages that can be opened, use *objtMessage*. Press Enter.

7. Right-click the newly created key, and then click **Modify**.

8. In the **Value data** box, type the number of objects that you want to limit this entry to, and then click **OK**. For example, type **350** to increase the value for the object.

9. Restart the Microsoft Exchange Information Store service.

## Item Size Limits

Item size limits are the limits placed on items within a user's mailbox. They are configurable by using the *MaxSendSize* and *MaxReceiveSize* parameters on the [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Set-Mailbox) cmdlet.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Item type</th>
<th>Limit</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Message (saved)</p></td>
<td><p>Maximum size of the SendLimit, ReceiveLimit</p></td>
</tr>
<tr class="even">
<td><p>Message (sent)</p></td>
<td><p>Maximum size of the SendLimit</p></td>
</tr>
<tr class="odd">
<td></td>
<td></td>
</tr>
</tbody>
</table>
