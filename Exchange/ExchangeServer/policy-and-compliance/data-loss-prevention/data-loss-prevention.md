---
localization_priority: Normal
description: 'Summary: Learn about DLP policies in on-premises Exchange Server 2016 and Exchange Server 2019, including what they contain and how to test them.'
ms.topic: article
author: chrisda
ms.author: chrisda
ms.assetid: 7c8ed3c1-ca91-4d9b-b16b-0a2b8ac89730
ms.date: 7/8/2018
ms.reviewer: 
title: Data loss prevention in Exchange Server
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Data loss prevention in Exchange Server

Data loss prevention (DLP) is important in Exchange Server because business critical email communication often includes sensitive data. DLP features make managing sensitive data in email messages easier than ever before by balancing compliance requirements without unnecessarily hindering the productivity of workers. For a conceptual overview of DLP, watch the following video.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/31f2b48e-93ed-4be3-b46d-e7230c0fed8f?autoplay=false]

DLP policies are simple packages that are collections of mail flow rules (also known as transport rules) that contain specific conditions, actions, and exceptions that filter messages and attachments based on their content. You can create a DLP policy, yet choose to not activate it. This allows you to test your policies without affecting mail flow. For more information, see [Test a mail flow rule](http://technet.microsoft.com/library/3d949e2a-8ba4-4261-8cfb-736fd2446ea1.aspx).

 DLP policies can use the full power of mail flow rules to detect and then act on messages in transit. For example, a mail flow rule can perform deep content analysis through keyword matches, dictionary matches, text pattern matches through regular expressions, and other content examination techniques to detect content that violates your organization's DLP policies. Document fingerprinting is also available to help you detect sensitive information in standard forms. For more information, see the following topics:

- [Document fingerprinting](http://technet.microsoft.com/library/1e0c579c-26e0-462a-a1b0-d7506dfe05fa.aspx)

- [Mail flow rules in Exchange Server](../../policy-and-compliance/mail-flow-rules/mail-flow-rules.md)

- [Integrating classification rules with mail flow rules](http://technet.microsoft.com/library/feb014a7-89dd-4f2d-a06d-52806ce435d4.aspx)

- [Messaging policy and compliance cmdlets](http://technet.microsoft.com/library/78ed4e33-f031-40fe-b632-9b15e3234e77.aspx)

In addition to the customizable DLP policies themselves, you can also inform email senders when they're about to violate one of your policies, even before they send a message that contains sensitive information. You do this by configuring Policy Tips. Policy Tips present a brief note about the possible policy violations in Outlook 2013 or later, Outlook on the web (formerly known as Outlook Web App), and Outlook on the web for devices. For more information, see [Policy Tips](http://technet.microsoft.com/library/4266b83c-dd8a-4b3d-99ff-402e68fc810c.aspx).

 **Notes:**

- DLP is a premium feature that requires an Exchange Enterprise Client Access License (CAL). For more information about CALs and server licensing, see [Exchange Server Licensing](https://go.microsoft.com/fwlink/p/?linkid=237292).

- In hybrid environments where some mailboxes are in on-premises Exchange and some are in Exchange Online, DLP policies are only applied in Exchange Online. Messages that are sent between on-premises users don't have DLP policies applied, because the messages don't leave the on-premises environment.

Looking for management tasks related to Data Loss Prevention? See [DLP Procedures](http://technet.microsoft.com/library/e2f575aa-552e-4dcc-8d7b-1ffd697d67df.aspx).

## Establish policies to protect sensitive data
<a name="dlp_establish"> </a>

The data loss prevention features can help you identify and monitor many categories of sensitive information that you have defined within the conditions of your policies, such as private identification numbers or credit card numbers. You have the option of defining your own custom policies and mail flow rules, or you can use the DLP policy templates that are included in Exchange to get started quickly. A *policy template* is a model that includes a range of conditions, rules, and actions that you can choose from to create and save an actual DLP policy that will help you inspect messages. For more information about the included policy templates, see [DLP Policy Templates Supplied in Exchange](http://technet.microsoft.com/library/7e1917ab-1920-4a52-97d1-7dfe2add6198.aspx).

There are three different methods that you can use to implement DLP:

- **Apply an out-of-the-box template supplied in Exchange**: The quickest way to start using DLP policies is to create and implement a new policy by using a template. This saves you the effort of building a new set of rules from nothing. You need to know what type of data you want to check for or which compliance regulation you're attempting to address. You also need to know your organization's expectations for processing this data. For more information, see [DLP Policy Templates Supplied in Exchange](http://technet.microsoft.com/library/7e1917ab-1920-4a52-97d1-7dfe2add6198.aspx) and [Create a DLP Policy From a Template](http://technet.microsoft.com/library/4432ef8b-6108-48d3-b2af-43ef5b40d2bc.aspx).

- **Import a pre-built policy file from outside your organization**: You can import policies that were created by independent software vendors. In this way, you can extend the DLP solution to meet your business requirements. For more information, see [Policies from Microsoft Partners](http://technet.microsoft.com/library/0f95336e-b3ef-4041-9604-adf7b0b335fe.aspx), [Define Your Own DLP Templates and Information Types](http://technet.microsoft.com/library/f4622dba-3347-4758-b4a2-f01b043c908c.aspx), and [Import a DLP Policy From a File](http://technet.microsoft.com/library/83f49dbd-f9b1-498e-b548-1529c5e1ccdb.aspx).

- **Create a custom policy without any pre-existing conditions**: Your enterprise may have its own requirements for monitoring certain types of data that's known to exist within a messaging system. You can create a custom policy entirely on your own to find and act on your own unique message data. You need to know the requirements and constraints of the environment where the DLP policy will be enforced to create effective custom policies. For more information, see [Create a Custom DLP Policy](http://technet.microsoft.com/library/b3299a39-9663-41e4-b76e-9d2f7879d486.aspx).

After you add a policy, you can review and change its rules, deactivate the policy, or remove it completely. For more information, see [Manage DLP Policies](http://technet.microsoft.com/library/ba81fabd-7f7f-4ef7-968f-ce851ada9d70.aspx).

## Sensitive information types in DLP policies
<a name="dlp_senstypes"> </a>

When you create or change DLP policies, you can include rules that look for sensitive information. The sensitive information types that are listed in the topic [Sensitive information types in Exchange Server](sensitive-information-types.md) are available for you to use in your policies. You can customize the conditions within a policy, such as how many times something has to be found before an action is taken, or the action to take. For more information about creating DLP policies see, [Create a Custom DLP Policy](http://technet.microsoft.com/library/b3299a39-9663-41e4-b76e-9d2f7879d486.aspx). For more information about mail flow rules, see [Mail flow rules in Exchange Server](../../policy-and-compliance/mail-flow-rules/mail-flow-rules.md).

To make it easy for you to use rules that look for sensitive information, Exchange comes with policy templates that already include some of the sensitive information types. You can't add conditions for all of the sensitive information types, because the templates are designed to help you focus on the most common types of compliance-related data within your organization. For more information about the pre-built templates, see [DLP Policy Templates Supplied in Exchange](http://technet.microsoft.com/library/7e1917ab-1920-4a52-97d1-7dfe2add6198.aspx).

 You can create many DLP policies for your organization, and enable them all so that many different types of information are looked for. You can also create a DLP policy that isn't based on an existing template. To create such a policy, see [Create a Custom DLP Policy](http://technet.microsoft.com/library/b3299a39-9663-41e4-b76e-9d2f7879d486.aspx). For more information about the available sensitive information types, see [Sensitive information types in Exchange Server](sensitive-information-types.md).

## Detecting sensitive form data with Document Fingerprinting
<a name="dlp_fingerprinting"> </a>

Exchange lets you use [Document Fingerprinting](http://technet.microsoft.com/library/1e0c579c-26e0-462a-a1b0-d7506dfe05fa.aspx) to easily create a sensitive information type that's based on a standard form. To learn how to protect form data, see [Protect Form Data with Document Fingerprinting](http://technet.microsoft.com/library/110c839b-7693-42f6-aa5d-58ce64f4c357.aspx).

## Policy Tips notify users about sensitive content expectations
<a name="dlp_tips"> </a>

You can use Policy Tip notification messages to inform email senders about possible compliance issues while they are composing an email message. When you configure a Policy Tip in a DLP policy, the notification message will only show up if something in the sender's email message matches the conditions described in your policy. Policy Tips are similar to MailTips that were introduced in Exchange 2010. For more information, see [Policy Tips](http://technet.microsoft.com/library/4266b83c-dd8a-4b3d-99ff-402e68fc810c.aspx).

## Detecting sensitive information along with traditional message classification
<a name="dlp_detectingsens"> </a>

A key factor in the strength of a DLP solution is the ability to correctly identify confidential or sensitive content that may be unique to your organization, regulatory needs, geography, or other business needs. The Exchange DLP architecture uses deep content analysis coupled with detection criteria that you establish through rules in your DLP policies. Helping to prevent data loss in Exchange requires you to configure the appropriate set of sensitive information rules that provide a high degree of protection while minimizing disruptions to mail flow that are caused by false positives and negatives. These types of rules (referred to throughout the DLP information as *sensitive information detection*) function within the framework of mail flow rules to enable DLP capabilities. To learn more about these features, see [Integrating sensitive information rules with mail flow rules](http://technet.microsoft.com/library/feb014a7-89dd-4f2d-a06d-52806ce435d4.aspx).

You can still apply traditional message classifications to messages, and you can combine these classifications with sensitive information detection. You can use these features together within a single DLP policy, or operate them independently (concurrently). To learn more about the traditional Exchange 2010 message classifications, see [Understanding Message Classifications](https://go.microsoft.com/fwlink/p/?LinkId=266612).

## Information about DLP-processed messages
<a name="dlp_information"> </a>

To see information about messages that contain DLP policy detections in your environment, see [DLP policy detection reports](http://technet.microsoft.com/library/5c3f1cf6-d8c7-4d83-bb24-641ea9d50cbc.aspx) and [Create incident reports for DLP policy detections](http://technet.microsoft.com/library/8e807f94-384c-43f5-be6f-06c5587175a0.aspx). Data related to DLP detections is highly integrated in the delivery reports.

## For more information
<a name="dlp_moreinfo"> </a>

- [Messaging policy and compliance in Exchange Server](../../policy-and-compliance/policy-and-compliance.md)

- [DLP Procedures](http://technet.microsoft.com/library/e2f575aa-552e-4dcc-8d7b-1ffd697d67df.aspx)

- [DLP policy detection reports](http://technet.microsoft.com/library/5c3f1cf6-d8c7-4d83-bb24-641ea9d50cbc.aspx)

- [Document Fingerprinting](http://technet.microsoft.com/library/1e0c579c-26e0-462a-a1b0-d7506dfe05fa.aspx)

- [Messaging Policy and Compliance Cmdlets](http://technet.microsoft.com/library/78ed4e33-f031-40fe-b632-9b15e3234e77.aspx)
