---
localization_priority: Normal
description: 'Summary: Learn about the Managed Store limits in Exchange Server 2016 and 2019 and how to change them.'
ms.topic: overview
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: bea9ec15-bfb5-4716-b14e-010e389c9f9e
ms.reviewer: 
title: Managed Store Limits  in Exchange 2016 and Exchange 2019
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Managed Store Limits in Exchange Server

The Managed Store in Exchange Server 2016 and Exchange Server 2019 is the name for the Information Store (also known as the Store) processes that manages mailbox databases. The Managed Store has connection and usage limits that prevent a single application or a single user from using all of the available connections, which could result in downtime. This topic describes the limits and how you can change them.

For more information about the Managed Store, see [Managed Store in Exchange Server](managed-store.md).

> [!NOTE]
> Connections by administrator accounts have maximum session limits of 64000. <br/><br/> Exchange Online limits (including Managed Store limits) are described in the [Exchange Online Limits](https://docs.microsoft.com/office365/servicedescriptions/exchange-online-service-description/exchange-online-limits).

## Terminology

Knowledge of the following terms will help you understand the types of connections referenced in this topic.

- **Sessions**

  Sessions represent the connections used by services and client applications (for example, Microsoft Outlook) to connect to the Managed Store. Services and clients can have multiple sessions at a particular time. The terms *connections* and *sessions* can be used interchangeably.

- **Threads**

  Threads represent concurrently executing requests to the Managed Store. For example, if a user opens a folder in Outlook, Outlook executes a request to the Managed Store on behalf of the user. That execution of the request is a single thread.

  For all clients, the maximum number of threads **per mailbox database** is 50. The exception is the Availability service, which has a maximum limit of 16 per user.

## Session limits

Session limits are based on connections per mailbox database on the server.

The types of connection limits are:

- **Max sessions per process**: The maximum number of sessions that an Exchange service can have open at one time on a mailbox database.

- **Max user sessions per process**: The maximum number of sessions for a specific protocol for a single user.

The types of client connections to the Managed Store and the limits based on those connections are described in the following table.

|**Client type**|**Max sessions per mailbox database**|**Default number of user sections per mailbox database**|
|:-----|:-----|:-----|
|Admin|10000|n/a|
|Availability service|10000|16|
|Content indexing|10000|n/a|
|Exchange ActiveSync|n/a|16|
|Exchange Web Services|n/a|16|
|Management|n/a|16|
|MAPI on the Middle Tier (MoMT)|n/a|32|
|MSExchangeMailboxAssistants: Events|10000|n/a|
|MSExchangeMailboxAssistants: Timed|10000|n/a|
|MSExchange Remote Procedure Call|n/a|16|
|Outlook on the web (formerly known as Outlook Web App)|n/a|16|
|POP3 and IMAP4|n/a|16|
|Transport|10000|n/a|
|Unified Messaging (Exchange 2016 only)|n/a|16|
|Others|n/a|16|

Use the following procedure to modify the default session limits.

**Notes**:

- When you modify a session limit, you need to modify that limit on all Mailbox servers within a database availability group (DAG). If you don't make the same changes on all servers, the results will be inconsistent.

- To increase a session limit in the Client Access (frontend) services, you need to use the [Set-ThrottlingPolicy](https://docs.microsoft.com/powershell/module/exchange/set-throttlingpolicy) cmdlet in the Exchange Management Shell.

> [!WARNING]
> Incorrectly editing the registry can cause serious problems that may require you to reinstall your operating system. Problems resulting from editing the registry incorrectly may not be able to be resolved. Before editing the registry, back up any valuable data.

1. Open the Registry Editor. For example, press Windows key + R, and then run **regedit**.

2. Go to the following location in the registry:

    **\\\\HKEY\_LOCAL\_MACHINE \\SYSTEM\\CurrentControlSet\\Services\\MSExchangeIS\\ParametersSystem**.

3. Select the **ParametersSystem** subkey, click **Edit** \> **New**, and then select **DWORD (32-bit) Value**.

    The new value is created as **New Value #1** in the right pane.

4. Rename the new key to one of the following values, and then press Enter:

      - **Maximum Allowed Sessions Per User**: This limit specifies the maximum allowable sessions per user.

      - **Maximum Allowed Service Sessions Per User**: This limit specifies the maximum allowed service sessions per user.

      - **Maximum Allowed Exchange Sessions Per Service**: This limit specifies the maximum allowed Exchange sessions per service. The default value is 10,000.

5. Select the new key, and then click **Edit** \> **Modify**.

6. In the dialog that opens, switch the **Base** value to **Decimal** and enter the new session limit in the **Value data** field.

   When you're finished, click **OK**.

## Open item limits

Open item limits are limits placed on the number of items that can be opened by a single mailbox in a single session. However, a user can have multiple sessions opened simultaneously. For example, if a user has two sessions opened, the user could open 1,000 folders.

The open item limits are described in the following table

|**Item type**|**Registry object type**|**Max opened per session**|
|:-----|:-----|:-----|
|ACL View|objtACLView|500|
|Attachment|objtAttachment|500|
|Attachment View|objtAttachmentView|500|
|CStream |objtCStream|Not applicable|
|Folder|objtFolder|500|
|Folder View|objtFolderView|500|
|FX Destination Stream|objtFXDstStrm|500|
|FX Source Stream|objtFXSrcStrm|500|
|Message|objtMessage|250|
|Message View|objtMessageView|500|
|Notification|objtNotify|500,000|
|Rule View|objtRulesView|Not applicable|
|Stream|objtStream|250|

You can limit the maximum number of resources that a MAPI client (for example, Outlook) can use simultaneously.

**Note**: When you modify a session limit, you need to modify that limit on all Mailbox servers within a database availability group (DAG). If you don't make the same changes on all servers, the results will be inconsistent.

> [!WARNING]
> Incorrectly editing the registry can cause serious problems that may require you to reinstall your operating system. Problems resulting from editing the registry incorrectly may not be able to be resolved. Before editing the registry, back up any valuable data.

1. Open the Registry Editor. For example, press Windows key + R, and then run **regedit**.

2. Go to the following location in the registry:

    **\\\\HKEY\_LOCAL\_MACHINE \\SYSTEM\\CurrentControlSet\\Services\\MSExchangeIS\\ParametersSystem**

3. Select the **ParametersSystem** subkey, click **Edit** \> **New**, and then select **Key**.

    The new value is created as **New Key #1** in the left pane.

4. Rename the new key to **MaxObjsPerMapiSession**, and then press Enter.

5. Select the **MaxObjsPerMapiSession** subkey, click **Edit** \> **New**, and then select **DWORD (32-bit) Value**.

   The new key is created as **New Value #1** in the right pane.

6. Rename the key to match one of the **Registry object type** values in the table. For example, to modify the number of messages that can be opened, enter *objtMessage* and then press Enter.

7. Select the new key, and then click **Edit** \> **Modify**.

8. In the dialog that opens, switch the **Base** value to **Decimal** and enter the new limit in the **Value data** field. For example, enter **350** to increase the value for *objtMessage*.

   When you're finished, click **OK**.

9. Restart the Microsoft Exchange Information Store service by running the following command in Windows PowerShell or the Exchange Management Shell:

   ```PowerShell
   Restart-Service MSExchangeIS
   ```

## Item size limits

Item size limits are the limits placed on items within a user's mailbox. You configure these limits by using the *MaxSendSize* and *MaxReceiveSize* parameters on the [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/set-mailbox) cmdlet in the Exchange Management Shell.

|**Item type**|**Limit**|
|:-----|:-----|
|Message (saved)|Maximum size of the SendLimit, ReceiveLimit|
|Message (sent)|Maximum size of the SendLimit|
