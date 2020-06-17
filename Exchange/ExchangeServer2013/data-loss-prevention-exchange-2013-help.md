---
title: 'Data loss prevention: Exchange 2013 Help'
TOCTitle: Data loss prevention
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 7c8ed3c1-ca91-4d9b-b16b-0a2b8ac89730
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Data loss prevention in Exchange 2013

_**Applies to:** Exchange Server 2013_

Learn about DLP policies in Exchange Server 2013, including what they contain and how to test them. You'll also learn about a new feature in Exchange DLP.

Data loss prevention (DLP) is an important issue for enterprise message systems because of the extensive use of email for business critical communication that includes sensitive data. In order to enforce compliance requirements for such data, and manage its use in email, without hindering the productivity of workers, DLP features make managing sensitive data easier than ever before. For a conceptual overview of DLP, watch the following video.

[DLP Overview](https://www.microsoft.com/videoplayer/embed/31f2b48e-93ed-4be3-b46d-e7230c0fed8f?autoplay=false)

DLP policies are simple packages that contain sets of conditions, which are made up of transport rules, actions, and exceptions that you create in the Exchange admin center (EAC) and then activate to filter email messages and attachments. You can create a DLP policy, but choose to not activate it. This allows you to test your policies without affecting mail flow. DLP policies can use the full power of existing transport rules. In fact, a number of new types of transport rules have been created in Microsoft Exchange Server 2013 in order to accomplish new DLP capability.

One important new feature of transport rules is a new approach to classifying sensitive information that can be incorporated into mail flow processing. This new DLP feature performs deep content analysis through keyword matches, dictionary matches, regular expression evaluation, and other content examination to detect content that violates organizational DLP policies. Exchange 2013 Service Pack 1 (SP1) adds [Document Fingerprinting](overview-of-document-fingerprinting-in-exchange.md), which helps you detect sensitive information in standard forms. For more information about transport rules, see [Transport rules in Exchange 2013](mail-flow-rules-transport-rules-in-exchange-2013-exchange-2013-help.md), and [Integrating sensitive information rules with transport rules](integrate-sensitive-information-rules-exchange-2013-help.md). You can also manage your DLP policies by using Exchange Management Shell cmdlets. For more information about policy and compliance cmdlets, see [Messaging policy and compliance](messaging-policy-and-compliance-exchange-2013-help.md).

In addition to the customizable DLP policies themselves, you can also inform email senders that they may be about to violate one of your policies, even before they send an offending message. You can accomplish this by configuring Policy Tips. Policy Tips are similar to MailTips, and can be configured to present a brief note in Outlook 2013 or later that provides information about possible policy violations to a person creating a message. In Exchange 2013 SP1, Policy Tips are also displayed in Outlook Web App and OWA for Devices. For more information, see [Policy Tips](policy-tips-exchange-2013-help.md).

> [!NOTE]
> DLP is a premium feature that requires an Exchange Enterprise Client Access License (CAL). For more information about CALs and server licensing, see [Exchange licensing FAQs](https://www.microsoft.com/microsoft-365/exchange/microsoft-exchange-server-licensing-licensing-overview). <br/><br/> **Exchange Enterprise CAL with Services**: There is a behavior distinction to take note of if you are an Exchange Enterprise CAL with Services customer with a hybrid deployment, where you have some mailboxes located on premises and some in Exchange Online. DLP policies are applied in Exchange Online. Therefore, messages sent from one on-premises user to another on-premises user do not have DLP policies applied, because the message doesn't leave the on-premises infrastructure.

Looking for management tasks related to Data Loss Prevention? See [DLP procedures](dlp-procedures-exchange-2013-help.md).

## Establish policies to protect sensitive data

The data loss prevention features can help you identify and monitor many categories of sensitive information that you have defined within the conditions of your policies, such as private identification numbers or credit card numbers. You have the option of defining your own custom policies and transport rules or using the pre-defined DLP policy templates provided by Microsoft in order to get started quickly. For more information about the policy templates that are included, see  [DLP policy templates supplied in Exchange 2013](built-in-dlp-policy-templates-exchange-2013-help.md). A policy template includes a range of conditions, rules, and actions that you can choose from in order to create and save an actual DLP policy that will help you inspect messages. The policy templates are models from which you can select or build your own specific rules to create a policy that meets your needs for data loss prevention.

Three different methods exist for you to begin using DLP:

1. **Apply an out-of-the-box template supplied by Microsoft**: The quickest way to start using DLP policies is to create and implement a new policy using a template. This saves you the effort of building a new set of rules from nothing. You will need to know what type of data you want to check for or which compliance regulation you are attempting to address. You will also need to know your organizations expectations for processing such data. More information at  [DLP policy templates supplied in Exchange 2013](built-in-dlp-policy-templates-exchange-2013-help.md) and [Create a DLP policy from a template](create-dlp-policy-from-template-exchange-2013-help.md).

2. **Import a pre-built policy file from outside your organization**: You can import policies that have already been created outside of your messaging environment by independent software vendors. In this way you can extend the DLP solutions to suit your business requirements. More information at [Policy templates from Microsoft partners](policy-templates-from-microsoft-partners-exchange-2013-help.md), [Define your own DLP templates and information types](define-your-own-dlp-templates-and-information-types-exchange-2013-help.md), and [Import a custom DLP policy template from a file](import-a-custom-dlp-policy-template-from-a-file-exchange-2013-help.md).

3. **Create a custom policy without any pre-existing conditions**: Your enterprise may have its own requirements for monitoring certain types of data known to exist within a messaging system. You can create a custom policy entirely on your own in order to start checking and acting upon your own unique message data. You will need to know the requirements and constraints of the environment in which the DLP policy will be enforced in order to create such a custom policy. More information at [Create a custom DLP policy](create-custom-dlp-policy-exchange-2013-help.md).

After you have added a policy, you can review and change its rules, make the policy inactive, or remove it completely. The procedures for these actions are provided in the [Manage DLP policies](manage-dlp-policies-exchange-2013-help.md) topic.

## Sensitive information types in DLP policies

When you create or change DLP policies, you can include rules that include checks for sensitive information. The sensitive information types listed in the [Sensitive information types in Exchange Server](https://docs.microsoft.com/Exchange/policy-and-compliance/data-loss-prevention/sensitive-information-types) topic are available to be used in your policies. The conditions that you establish within a policy, such as how many times something has to be found before an action is taken or exactly what that action is can be customized within your new custom policies in order to meet your specific policy requirements. For more information about creating DLP policies see, [Create a custom DLP policy](create-custom-dlp-policy-exchange-2013-help.md). For more information about the full suite transport rules, see [Transport rules in Exchange 2013](mail-flow-rules-transport-rules-in-exchange-2013-exchange-2013-help.md).

To make it easy for you to make use of the sensitive information-related rules, Microsoft has supplied policy templates that already include some of the sensitive information types. You cannot add conditions for all of the sensitive information types listed here to policy templates however, because the templates are designed to help you focus on the most-common types of compliance-related data within your organization. For more information about the pre-built templates, see  [DLP policy templates supplied in Exchange 2013](built-in-dlp-policy-templates-exchange-2013-help.md). You can create numerous DLP policies for your organization and have them all enabled so that many disparate types of information are examined. You can also create a DLP policy that is not based on an existing template. To begin creating such a policy, see [Create a custom DLP policy](create-custom-dlp-policy-exchange-2013-help.md). For more information about sensitive information types, see [Sensitive information types in Exchange Server](https://docs.microsoft.com/Exchange/policy-and-compliance/data-loss-prevention/sensitive-information-types).

## Detecting sensitive form data with Document Fingerprinting

With Exchange 2013 SP1, you can use [Document Fingerprinting](overview-of-document-fingerprinting-in-exchange.md) to easily create a sensitive information type based on a standard form. To learn how to protect form data, see [Protect form data with document fingerprinting](protect-data-with-fingerprinting-exchange-2013-help.md).

## Policy Tips notify users about sensitive content expectations

You can use Policy Tip notification messages to inform email senders about possible compliance issues while they are composing an email message. When you configure a Policy Tip in a DLP policy, the notification message will only show up if something in the sender's email message meets the conditions described in your policy. Policy Tips are similar to MailTips that were introduced in Microsoft Exchange 2010. For more information, see [Policy Tips](policy-tips-exchange-2013-help.md).

## Detecting sensitive information along with traditional message classification

Exchange 2013 presents a new method of helping you manage message and attachment data when compared with traditional message classification. A key factor in the strength of a DLP solution is the ability to correctly identify confidential or sensitive content that may be unique to the organization, regulatory needs, geography, or other business needs. Exchange 2013 can achieve this by using a new architecture for deep content analysis coupled with detection criteria that you establish through rules in your DLP policies. Helping prevent data loss in Exchange 2013 relies on configuring the correct set of sensitive information rules so that they provide a high degree of protection while minimizing inappropriate mail flow disruption with false positives and negatives. These types of rules, referred to throughout the DLP information as sensitive information detection, function within the framework offered by transport rules in order to enable DLP capabilities.

To learn more about these new features, see [Integrating sensitive information rules with transport rules](integrate-sensitive-information-rules-exchange-2013-help.md). The traditional message classification fields can still be applied to messages in Exchange and these can be combined with the new sensitive information detection either together within a single DLP policy or running concurrently so they are evaluated independently within Exchange. To learn more about the legacy Exchange 2010 message classifications, see [Understanding Message Classifications](https://go.microsoft.com/fwlink/p/?LinkId=266612).

## Information about DLP-processed messages

For Exchange 2013 to obtain information about messages and DLP policy detections in your environment, see [View DLP policy detection reports](view-dlp-policy-detection-reports-exchange-2013-help.md) and [Create incident reports for DLP policy detections](create-incident-reports-for-dlp-policy-detections-exchange-2013-help.md). Data related to DLP detections, is highly integrated into the delivery reports message tracking tool of Exchange 2013.

## Installation prerequisites

In order to make use of DLP features, you must have Exchange 2013 configured with at least one sender mailbox. Data Loss Prevention is a premium feature that requires an Enterprise Client Access License (CAL). For more information about getting started with Exchange Server, see [Planning and deployment](planning-and-deployment-for-exchange-2013-installation-instructions.md).

## For more information

- [Messaging policy and compliance](messaging-policy-and-compliance-exchange-2013-help.md)

- [DLP procedures](dlp-procedures-exchange-2013-help.md)

- [View DLP policy detection reports](view-dlp-policy-detection-reports-exchange-2013-help.md)

- [Document Fingerprinting](overview-of-document-fingerprinting-in-exchange.md)
