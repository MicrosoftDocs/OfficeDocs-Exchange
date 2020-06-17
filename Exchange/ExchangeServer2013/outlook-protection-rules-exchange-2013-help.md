---
title: 'Outlook protection rules: Exchange 2013 Help'
TOCTitle: Outlook protection rules
ms:assetid: bd7d0ad7-1f8e-46da-a74b-58c58f3eff93
ms:mtpsurl: https://technet.microsoft.com/library/Dd638178(v=EXCHG.150)
ms:contentKeyID: 49319928
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Outlook protection rules

_**Applies to:** Exchange Server 2013_

Every day, information workers exchange sensitive information by email, including financial reports and data, customer and employee information, and confidential product information and specifications. In Microsoft Exchange Server 2013, Microsoft Outlook, and Microsoft Office Outlook Web App, users can apply Information Rights Management (IRM) protection to messages by applying an Active Directory Rights Management Services (AD RMS) rights policy template. This requires an AD RMS deployment in the organization. For more information about AD RMS, see [Active Directory Rights Management Services](https://go.microsoft.com/fwlink/p/?linkid=129823).

However, when left to the discretion of users, messages may be sent in clear text without IRM protection. In organizations that use email as a hosted service, there's a risk of information leakage as a message leaves the client and is routed and stored outside the boundaries of an organization. Although email hosting companies may have well-defined procedures and checks to help mitigate the risk of information leakage, after a message leaves the boundary of an organization, the organization loses control of the information. Outlook protection rules can help protect against this type of information leakage.

For management tasks related to managing IRM, see [Information Rights Management procedures](information-rights-management-procedures-exchange-2013-help.md).

## Automatic IRM protection in Outlook

In Exchange 2013, Outlook protection rules help your organization protect against the risk of information leakage by automatically applying IRM-protection to messages in Exchange 2013. Messages are IRM-protected before they leave the Outlook client. This protection is also applied to any attachments using supported file formats.

When you create Outlook protection rules on an Exchange 2013 server, the rules are automatically distributed to Outlook 2010 by using Exchange Web Services. For Outlook 2010 to apply the rule, the AD RMS rights policy template you specify must be available on users' computers.

> [!IMPORTANT]
> If a rights policy template is removed from the AD&nbsp;RMS server, you must modify any Outlook protection rules that use the removed template. If an Outlook protection rule continues to use a rights policy template that's been removed, and transport decryption is enabled in the organization, the Decryption agent will fail to decrypt the message protected with a template that's no longer available. If transport decryption is configured as mandatory, the Transport service will reject the message and send a non-delivery report (NDR) to the sender. For more details about transport decryption, see <A href="transport-decryption-exchange-2013-help.md">Transport decryption</A>. For more details about AD&nbsp;RMS rights policy templates, see <A href="https://go.microsoft.com/fwlink/p/?linkid=179455">AD RMS Policy Template Considerations</A>.<BR>In Windows Server 2008 and later, rights policy templates can be archived instead of deleted. Archived templates can still be used to license content, but when you create or modify an Outlook protection rule, archived templates aren't included in the list of templates.

Outlook protection rules are similar to transport protection rules. Both are applied based on message conditions, and both protect messages by applying an AD RMS rights protection template. However, transport protection rules are applied in the Transport service on the Mailbox server by the Transport Rules agent. Outlook protection rules are applied in Outlook 2010, before the message leaves the user's computer. Messages protected by an Outlook protection rule enter the transport pipeline with IRM protection already applied. Additionally, messages protected with an Outlook protection rule are also saved in an encrypted format in the Sent Items folder of the sender's mailbox.

> [!NOTE]
> If transport decryption is enabled in your Exchange organization, messages that are IRM-protected by an Outlook protection rule using the AD&nbsp;RMS server in your organization can be decrypted by the Decryption agent on Transport service. Message content can be inspected by the Transport Rules agent and other transport agents installed on the Transport service. For more details about transport decryption, see <A href="transport-decryption-exchange-2013-help.md">Transport decryption</A>.

When you use transport protection rules, users have no indication of whether a message is going to be automatically protected on the Transport service. When an Outlook protection rule is applied to a message in Outlook 2010, users know if a message will be IRM-protected. If required, users can also select a different rights policy template.

## Creating Outlook protection rules

To create Outlook protection rules, you must use the [New-OutlookProtectionRule](https://docs.microsoft.com/powershell/module/exchange/New-OutlookProtectionRule) cmdlet in the Exchange Management Shell. For detailed instructions, see [Create an Outlook Protection Rule](create-an-outlook-protection-rule-exchange-2013-help.md).

When creating a rule, you can specify whether the user can override it, either by removing IRM-protection or by applying a different AD RMS rights policy template than the one specified in the rule. If a user overrides the IRM protection applied by an Outlook protection rule, Outlook 2010 inserts the `X-MS-Outlook-Client-Rule-Overridden` header in the message, which allows you to determine that the rule was overridden by the user.

## Predicates in Outlook protection rules

Outlook protection rules allow you to use three predicates to automatically apply IRM protection in Outlook 2010:

  - **FromDepartment**: The *FromDepartment* predicate looks up the sender's department attribute in Active Directory and automatically IRM-protects the message if the sender's department matches the department specified in the rule. For example, you can create an Outlook protection rule to automatically protect all messages sent by the Research department.

  - **SentTo**: Your organization may need to protect messages sent to certain sensitive recipients, such as the All Company or Finance distribution groups. Using the *SentTo* predicate, you can create an Outlook protection rule to automatically IRM-protect messages sent to specified recipients.

  - **SentToScope**: The *SentToScope* predicate allows you to create an Outlook protection rule to automatically IRM-protect messages sent inside or outside the organization. For example, you can use the *SentToScope* predicate with the *FromDepartment* predicate to IRM-protect messages sent by a particular department to internal users.
