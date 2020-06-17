---
title: 'Create an Outlook Protection Rule: Exchange 2013 Help'
TOCTitle: Create an Outlook Protection Rule
ms:assetid: da64750d-faaf-44de-ad8c-888eba7fbdbf
ms:mtpsurl: https://technet.microsoft.com/library/Dd638196(v=EXCHG.150)
ms:contentKeyID: 49319935
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Create an Outlook Protection Rule

_**Applies to:** Exchange Server 2013_

Using Microsoft Outlook protection rules, you can protect messages with Information Rights Management (IRM) by applying an Active Directory Rights Management Services (AD RMS) template in Outlook 2010 before the messages are sent.

For additional management tasks related to IRM, see [Information Rights Management procedures](information-rights-management-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to completion: 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Rights protection" entry in the [Messaging policy and compliance permissions](messaging-policy-and-compliance-permissions-exchange-2013-help.md) topic.

- You must have an [AD RMS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831364(v=ws.11)x) server deployed in the same Active Directory forest as your server running Microsoft Exchange Server 2013.

- If you configure Outlook protection rules to IRM-protect messages, consider enabling transport decryption to allow transport agents, including the Transport Rules agent, to decrypt and access the message. If you use journaling, you should also consider enabling journal report decryption to allow the Journaling agent to save an unencrypted copy of the message in the journal report. For more information, see [Journal report decryption](journal-report-decryption-exchange-2013-help.md).

- You can't use the Exchange admin center (EAC) to create Outlook protection rules. You must use the Shell.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to create an Outlook protection rule

This example creates the Outlook protection rule Project Contoso. The rule protects messages sent to the ContosoPMs distribution group with the AD RMS template Business Critical.

```powershell
New-OutlookProtectionRule -Name "Project Contoso" -SentTo "DL-ContosoPMs@contoso.com" -ApplyRightsProtectionTemplate "Business Critical"
```

> [!NOTE]
> When you use the <CODE>SentTo</CODE> predicate for an Outlook protection rule and specify a distribution group, only messages addressed to the distribution group in the To, Cc, or Bcc fields are IRM-protected. IRM protection isn't applied to messages addressed to individual members of the distribution group.

You can also use the `FromDepartment` and `SentToScope` predicates to apply IRM protection to messages sent from users in the specified department or messages sent to the specified scope (`InOrganization` for internal messages, `All` for all recipients).

For detailed syntax and parameter information, see [New-OutlookProtectionRule](https://docs.microsoft.com/powershell/module/exchange/New-OutlookProtectionRule).

## How do you know this worked?

To verify that you have successfully created an Outlook protection rule, do the following:

- Run the [Get-OutlookProtectionRule](https://docs.microsoft.com/powershell/module/exchange/Get-OutlookProtectionRule) cmdlet to make sure that the rule has been created and to view the rule's properties. For an example of how to retrieve an Outlook protection rule, see [Examples](https://docs.microsoft.com/powershell/module/exchange/Get-OutlookProtectionRule#examples).

- Use Outlook 2010 to create a test message that meets the rule's condition and make sure the rule is triggered on the client.

    > [!NOTE]
    > It may take some time for an Outlook protection rule to be available in Outlook.
