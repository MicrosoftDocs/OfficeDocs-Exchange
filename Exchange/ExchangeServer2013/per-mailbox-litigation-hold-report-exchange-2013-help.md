---
title: 'Run a per-mailbox litigation hold report: Exchange 2013 Help'
TOCTitle: Run a per-mailbox litigation hold report
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 98c46226-2f48-42c6-a741-34bb5944f519
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Run a per-mailbox litigation hold report in Exchange 2013

_**Applies to:** Exchange Server 2013_

If your organization is involved in a legal action, you may have to take steps to preserve relevant data, such as email messages, that may be used as evidence. In situations like this, you can use litigation hold to retain all email sent and received by specific people or retain all email sent and received in your organization for a specific time period. For more information about what happens when a mailbox is on litigation hold and how to enable and disable it, see the "Mailbox Features" section in [Manage user mailboxes](manage-user-mailboxes-exchange-2013-help.md).

Use the litigation hold report to keep track of the following types of changes made to a mailbox in a given time period:

- Litigation hold was enabled.

- Litigation hold was disabled.

For each of these change types, the report includes the user who made the change and the time and date the change was made.

## What do you need to know before you begin?

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "View-only administrator audit logging" entry in the [Exchange and Shell infrastructure permissions](exchange-and-shell-infrastructure-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to run a litigation hold report

1. In the EAC, navigate to **Compliance Management** \> **Auditing**.

2. Click **Run a per-mailbox litigation hold report**.

    Microsoft Exchange runs the report for litigation hold changes made to any mailbox in the past two weeks.

3. To view the changes for a specific mailbox, in the search results pane, select the mailbox. View the search results in the details pane.

> [!TIP]
> Want to narrow the search results? Select the start date, end date, or both, and select specific mailboxes to search. Click **Search** to re-run the report.

## How do you know this worked?

To verify that you've successfully run a litigation hold report, mailboxes that had litigation hold changes within the date range are displayed in the search results pane. If there are no results, then no changes to litigation hold have taken place within the date range or recent changes haven't taken effect yet.

> [!NOTE]
> When a mailbox is put on litigation hold, it can take up to 60 minutes for the hold to take effect.
