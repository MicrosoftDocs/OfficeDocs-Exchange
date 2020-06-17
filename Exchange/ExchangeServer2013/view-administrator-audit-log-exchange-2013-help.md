---
title: 'View the administrator audit log: Exchange 2013 Help'
TOCTitle: View the administrator audit log
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 5c62072a-556d-4fea-9973-d668c6b9fd57
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# View the administrator audit log in Exchange 2013

_**Applies to:** Exchange Server 2013_

In Microsoft Exchange 2013, you can use the Exchange admin center (EAC) to search for and view entries in the administrator audit log. The administrator audit log records specific actions, based on Exchange Management Shell cmdlet, performed by administrators and users who have been assigned administrative privileges. Entries in the administrator audit log provide you with information about what cmdlet was run, which parameters were used, who ran the cmdlet, and what objects were affected.

> [!NOTE]
> Administrator auditing logging is enabled by default. <br/> The administrator audit log doesn't record any action that is based on an Exchange Management Shell cmdlet that begins with the verbs **Get**, **Search**, or **Test**. <br/> Audit log entries are kept for 90 days. When an entry is older than 90 days, it's deleted.

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "View reports" entry in the [Messaging policy and compliance permissions](messaging-policy-and-compliance-permissions-exchange-2013-help.md) topic.

- As previously stated, administrator audit logging is enabled by default. To verify that it's enabled, you can run the following command.

  ```powershell
  Get-AdminAuditLogConfig | Format-List AdminAuditLogEnabled
  ```

  You can enable administrator audit logging if it's disabled by running the following command.

  ```powershell
  Set-AdminAuditLogConfig -AdminAuditLogEnabled $True
  ```

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to view the administrator audit log

1. In the EAC, go to **Compliance management** \> **Auditing**, and choose **Run the admin audit log report**.

2. Choose a **Start date** and **End date**, and then choose **Search**. All configuration changes made during the specified time period are displayed, and can be sorted, using the following information:

   - **Date**: The date and time that the configuration change was made. The date and time are stored in Coordinated Universal Time (UTC) format.

   - **Cmdlet**: The name of the cmdlet that was used to make the configuration change.

   - **User**: The name of the user account of the user who made the configuration change.

   Up to 5000 entries will be displayed on multiple pages. Specify a smaller date range if you need to narrow your results. If you select an individual search result, the following additional information is displayed in the details pane:

   - **Object modified**: The object that was modified by the cmdlet.

   - **Parameters (Parameter:Value)**: The cmdlet parameters that were used, and any value specified with the parameter.

3. If you want to print a specific audit log entry, choose the **Print** button in the details pane.

## How do you know this worked?

If you've successfully run an administrator audit log report, configuration changes made within the date range you specify are displayed in the search results pane. If there are no results, change the date range and then run the report again.

> [!NOTE]
> When a change is made in your organization, it may take up to 15 minutes to appear in audit log search results. If a change doesn't appear in the administrator audit log, wait a few minutes and run the search again.
