---
localization_priority: Normal
description: Learn about DLP policies in Exchange Server and Exchange Online, including what they contain and how to test them. You'll also learn about a new feature in Exchange DLP.
ms.topic: overview
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 7c8ed3c1-ca91-4d9b-b16b-0a2b8ac89730
ms.reviewer: 
f1.keywords:
- NOCSH
title: Data loss prevention
ms.collection:
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Data loss prevention

Learn about DLP policies in Exchange Server and Exchange Online, including what they contain and how to test them. You'll also learn about a new feature in Exchange DLP.

Data loss prevention (DLP) is an important issue for enterprise message systems because of the extensive use of email for business critical communication that includes sensitive data. In order to enforce compliance requirements for such data, and manage its use in email, without hindering the productivity of workers, DLP features make managing sensitive data easier than ever before. For a conceptual overview of DLP, watch the following video.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/31f2b48e-93ed-4be3-b46d-e7230c0fed8f?autoplay=false]

DLP policies are simple packages that contain sets of conditions, which are made up of mail flow rule (also known as transport rule) conditions, exceptions, and actions that you create in the Exchange admin center (EAC) and then activate to filter email messages and attachments. You can create a DLP policy, but choose to not activate it. This allows you to test your policies without affecting mail flow. DLP policies can use the full power of existing mail flow rules. In fact, a number of new types of mail flow rules have been created in Microsoft Exchange Server and Exchange Online in order to accomplish new DLP capability. One important new feature of mail flow rules is a new approach to classifying sensitive information that can be incorporated into mail flow processing. This new DLP feature performs deep content analysis through keyword matches, dictionary matches, regular expression evaluation, and other content examination to detect content that violates organizational DLP policies. For more information about mail flow rules, see [Mail flow rules (transport rules) in Exchange Online](../mail-flow-rules/mail-flow-rules.md), and [Integrating sensitive information rules with mail flow rules in Exchange Online](integrate-sensitive-information-rules.md). You can also manage your DLP policies by using Exchange Online PowerShell cmdlets at [Exchange PowerShell](https://docs.microsoft.com/powershell/exchange/).

In addition to the customizable DLP policies themselves, you can also inform email senders that they may be about to violate one of your policies, even before they send an offending message. You can accomplish this by configuring Policy Tips. Policy Tips are similar to MailTips, and can be configured to present a brief note in the Microsoft Outlook 2013 client that provides information about possible policy violations to a person creating a message. In Exchange Online and in Exchange Server, Policy Tips are also displayed in Outlook on the web (formerly known as Outlook Web App) and OWA for Devices. For more information, see [Policy Tips](policy-tips.md).

> [!NOTE]
> Data Loss Prevention is a premium feature. For more information, see [Exchange Online Licensing](https://products.office.com/exchange/compare-microsoft-exchange-online-plans), [Exchange Online Service Description](https://docs.microsoft.com/office365/servicedescriptions/exchange-online-service-description/exchange-online-service-description), and [Exchange Online Protection Service Description](https://docs.microsoft.com/office365/servicedescriptions/exchange-online-protection-service-description/exchange-online-protection-service-description).

> [!NOTE]
> Messages sent between on-premises users in a hybrid deployment do not have Exchange Online DLP policies applied because the messages do not leave the on-premises infrastructure.

## Establish policies to protect sensitive data
<a name="dlp_establish"> </a>

The data loss prevention features can help you identify and monitor many categories of sensitive information that you have defined within the conditions of your policies, such as private identification numbers or credit card numbers. You have the option of defining your own custom policies and mail flow rules or using the pre-defined DLP policy templates provided by Microsoft in order to get started quickly. For more information about the policy templates that are included, see [DLP policy templates supplied in Exchange](dlp-policy-templates.md). A policy template includes a range of conditions, rules, and actions that you can choose from in order to create and save an actual DLP policy that will help you inspect messages. The policy templates are models from which you can select or build your own specific rules to create a policy that meets your needs for data loss prevention.

Three different methods exist for you to begin using DLP:

1. **Apply an out-of-the-box template supplied by Microsoft**: The quickest way to start using DLP policies is to create and implement a new policy using a template. This saves you the effort of building a new set of rules from nothing. You will need to know what type of data you want to check for or which compliance regulation you are attempting to address. You will also need to know your organizations expectations for processing such data. More information at [DLP policy templates supplied in Exchange](dlp-policy-templates.md) and [Create a DLP policy from a template](create-dlp-policy-from-template.md).

2. **Import a pre-built policy file from outside your organization**: You can import policies that have already been created outside of your messaging environment by independent software vendors. In this way you can extend the DLP solutions to suit your business requirements.

3. **Create a custom policy without any pre-existing conditions**: Your enterprise may have its own requirements for monitoring certain types of data known to exist within a messaging system. You can create a custom policy entirely on your own in order to start checking and acting upon your own unique message data. You will need to know the requirements and constraints of the environment in which the DLP policy will be enforced in order to create such a custom policy. More information at [Create a custom DLP policy](create-custom-dlp-policy.md).

After you have added a policy, you can review and change its rules, make the policy inactive, or remove it completely.

## Sensitive information types in DLP policies
<a name="dlp_senstypes"> </a>

When you create or change DLP policies, you can include rules that include checks for sensitive information. The sensitive information types listed in the [Sensitive information types in Exchange Server](https://docs.microsoft.com/Exchange/policy-and-compliance/data-loss-prevention/sensitive-information-types) topic are available to be used in your policies. The conditions that you establish within a policy, such as how many times something has to be found before an action is taken or exactly what that action is can be customized within your new custom policies in order to meet your specific policy requirements. For more information about creating DLP policies see, [Create a custom DLP policy](create-custom-dlp-policy.md). For more information about the full suite mail flow rules, see [Mail flow rules (transport rules) in Exchange Online](../../security-and-compliance/mail-flow-rules/mail-flow-rules.md).

To make it easy for you to make use of the sensitive information-related rules, Microsoft has supplied policy templates that already include some of the sensitive information types. You cannot add conditions for all of the sensitive information types listed here to policy templates however, because the templates are designed to help you focus on the most-common types of compliance-related data within your organization. For more information about the pre-built templates, see [DLP policy templates supplied in Exchange](dlp-policy-templates.md). You can create numerous DLP policies for your organization and have them all enabled so that many disparate types of information are examined. You can also create a DLP policy that is not based on an existing template. To begin creating such a policy, see [Create a custom DLP policy](create-custom-dlp-policy.md). For more information about sensitive information types, see [Sensitive information types in Exchange Server](https://docs.microsoft.com/Exchange/policy-and-compliance/data-loss-prevention/sensitive-information-types).

## Policy Tips notify users about sensitive content expectations
<a name="dlp_tips"> </a>

You can use Policy Tip notification messages to inform email senders about possible compliance issues while they are composing an email message. When you configure a Policy Tip in a DLP policy, the notification message will only show up if something in the sender's email message meets the conditions described in your policy. Policy Tips are similar to MailTips that were introduced in Microsoft Exchange 2010. For more information, see [Policy Tips](policy-tips.md).

## Detecting sensitive information along with traditional message classification
<a name="dlp_detectingsens"> </a>

Exchange Server and Exchange Online present a new method of helping you manage message and attachment data when compared with traditional message classification. A key factor in the strength of a DLP solution is the ability to correctly identify confidential or sensitive content that may be unique to the organization, regulatory needs, geography, or other business needs. Exchange Server can achieve this by using a new architecture for deep content analysis coupled with detection criteria that you establish through rules in your DLP policies. Helping prevent data loss in Exchange Server relies on configuring the correct set of sensitive information rules so that they provide a high degree of protection while minimizing inappropriate mail flow disruption with false positives and negatives. These types of rules, referred to throughout the DLP information as sensitive information detection, function within the framework offered by mail flow rules in order to enable DLP capabilities.

To learn more about these new features, see [Integrating sensitive information rules with mail flow rules in Exchange Online](integrate-sensitive-information-rules.md). The traditional message classification fields can still be applied to messages in Exchange and these can be combined with the new sensitive information detection either together within a single DLP policy or running concurrently so they are evaluated independently within Exchange. To learn more about the legacy Exchange 2010 message classifications, see [Understanding Message Classifications](https://docs.microsoft.com/previous-versions/office/exchange-server-2010/bb123498(v=exchg.141)).

## Installation prerequisites
<a name="dlp_install"> </a>

In order to make use of DLP features, you must have Exchange Online configured with at least one sender mailbox. Data Loss Prevention is a premium feature that requires an Enterprise Client Access License (CAL). For more information about getting started with Exchange Online, see [Exchange Online](../../exchange-online.md).

## For more information
<a name="dlp_moreinfo"> </a>

Exchange Online

- [Security and compliance for Exchange Online](../../security-and-compliance/security-and-compliance.md)
