---
title: 'Create a Transport Protection Rule: Exchange 2013 Help'
TOCTitle: Create a Transport Protection Rule
ms:assetid: 3a857185-ee16-4ee7-9e57-8be95f7e753a
ms:mtpsurl: https://technet.microsoft.com/library/Dd302432(v=EXCHG.150)
ms:contentKeyID: 49319907
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Create a Transport Protection Rule

_**Applies to:** Exchange Server 2013_

You can use transport protection rules to apply persistent rights protection to messages based on properties such as sender, recipient, message subject, and content.

> [!WARNING]
> Before you create transport rules in your production environment, we recommend creating them in a test environment and testing them thoroughly. The transport rules created in this topic are examples. You can create transport rules by using the appropriate transport rule predicates and values based on your requirements.

For additional management tasks related to Information Rights Management (IRM), see [Information Rights Management procedures](information-rights-management-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to completion: 2-5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Transport rules" entry in the [Messaging policy and compliance permissions](messaging-policy-and-compliance-permissions-exchange-2013-help.md) topic.

- A server running [Active Directory Rights Management Services (AD RMS)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831364(v=ws.11)) must be available in your organization and contain existing RMS templates.

- If you configure transport protection rules to protect messages using IRM, and you also use journaling, consider enabling journal report decryption to allow the Journaling agent to save an unencrypted copy of the message in the journal report. To learn more, see [Journal report decryption](journal-report-decryption-exchange-2013-help.md).

- After you create a transport protection rule, if the rule can't be applied to messages because an AD RMS server is unavailable, messages will be queued by the Transport service on Mailbox servers. Depending on the volume of these messages, additional disk space may be consumed on Mailbox servers. Exchange will attempt to IRM-protect the message three times. After these attempts, if the AD RMS server is unreachable or the message can't be IRM-protected, a non-delivery report (NDR) is sent to the sender.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the EAC to create a transport protection rule

1. Navigate to **Mail flow** \> **Rules**.

2. In the list view, click **New** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

3. In **New Rule**, first click **More options**, and then complete the following fields:

   - **Name**: Type a name for the transport rule.

   - **Apply this rule if**: Select a condition and enter any required values for the condition. To add more conditions, click **Add condition**.

     > [!IMPORTANT]
     > If you don't select any conditions when creating a transport protection rule, all messages handled by Exchange 2013 servers with the Transport service in your organization are IRM-protected. IRM-protecting all messages requires more resources. Therefore, we recommend that you plan your Mailbox server and AD&nbsp;RMS deployment accordingly.

   - **Do the following**: Select **Apply rights protection to the message with** and then use the **Select RMS template** dialog box to select a template.

   - **Except if**: (Optional) Click **Add exception** to specify an exception to the rule.

4. Click **Save** to create the transport rule.

## Use the Shell to create a transport protection rule

- To create a transport protection rule, you must have existing RMS templates in your AD RMS deployment. This example retrieves the available templates from your AD RMS cluster.

    ```powershell
    Get-RMSTemplate | format-list
    ```

  For detailed syntax and parameter information, see [Get-RMSTemplate](https://docs.microsoft.com/powershell/module/exchange/Get-RMSTemplate).

- This example creates the transport protection rule Protect-BusinessCriticalProject. The rule IRM-protects messages that contain the phrase "Business Critical" in the Subject field with the **Do Not Forward** template.

    > [!NOTE]
    > The <CODE>SubjectContainsWords</CODE> predicate is used in this example. You can use any combination of transport rule predicates to form the conditions and exceptions for the rule. For information about the available predicates, see <A href="mail-flow-rule-conditions-and-exceptions-predicates-in-exchange-2013-exchange-2013-help.md">Transport rule conditions (predicates)</A>.

    ```powershell
    New-TransportRule -Name "Protect-BusinessCriticalProject" -SubjectContainsWords "Business Critical" -ApplyRightsProtectionTemplate "Do Not Forward"
    ```

  For detailed syntax and parameter information, see [New-TransportRule](https://docs.microsoft.com/powershell/module/exchange/New-TransportRule).

## How do you know this worked?

To verify that you have successfully created a transport protection rule, do one of the following:

- Use the EAC to verify that the rule has been created, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon") to view the rule's properties.

- Use the [Get-TransportRule](https://docs.microsoft.com/powershell/module/exchange/Get-TransportRule) cmdlet to retrieve the rule. For an example of how to retrieve a rule, see [Examples](https://docs.microsoft.com/powershell/module/exchange/Get-TransportRule#examples) in **Get-TransportRule**.

- Using Outlook, Outlook Web App, or a mobile device, send a test message that meets the rule conditions and check whether the message received by the recipient is IRM-protected.
