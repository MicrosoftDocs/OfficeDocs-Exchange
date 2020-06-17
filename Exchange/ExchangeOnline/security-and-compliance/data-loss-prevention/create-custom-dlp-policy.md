---
localization_priority: Normal
description: A custom data loss prevention (DLP) policy allows you to establish conditions, rules, and actions that can help meet the specific needs of your organization, and which may not be covered in one of the pre-existing DLP templates.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: b3299a39-9663-41e4-b76e-9d2f7879d486
ms.reviewer: 
f1.keywords:
- NOCSH
title: Create a custom DLP policy
ms.collection:
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Create a custom DLP policy in Exchange Online

A custom data loss prevention (DLP) policy allows you to establish conditions, rules, and actions that can help meet the specific needs of your organization, and which may not be covered in one of the pre-existing DLP templates.

The rule conditions that are available to you in a single policy include all the traditional mail flow rules (also known as transport rules) in addition to the sensitive information types presented in [Sensitive information types in Exchange Server](https://docs.microsoft.com/Exchange/policy-and-compliance/data-loss-prevention/sensitive-information-types). For more information about mail flow rules, see [Mail flow rules (transport rules) in Exchange Online](../../security-and-compliance/mail-flow-rules/mail-flow-rules.md).

> [!CAUTION]
> You should enable your DLP policies in test mode before running them in your production environment. During such tests, it is recommended that you configure sample user mailboxes and send test messages that invoke your test policies in order to confirm the results. for more information about testing, see [Test a mail flow rule](../../security-and-compliance/mail-flow-rules/test-mail-flow-rules.md).

## What do you need to know before you begin?

- Estimated time to complete: 60 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Data loss prevention (DLP)" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!NOTE]
> Due to the variances in customer environments and content match requirements, Microsoft Support cannot assist in providing custom content matching definitions; e.g., defining Custom Classifications and/or Regular Expression patterns ("RegEx"). For custom content matching development, testing, and debugging, customers will need to rely upon internal IT resources, or use an external consulting resource such as Microsoft Consulting Services (MCS). Support engineers can provide limited support for the feature, but cannot provide assurances that any custom content matching development will fulfill the customer's requirements or obligations. As an example of the type of support which can be provided, sample regular expression patterns may be provided for testing purposes, or support can assist with troubleshooting an existing RegEx pattern which is not triggering as expected with a single specific content example.

For additional information on the .NET regex engine which is used for processing the text, see https://docs.microsoft.com/dotnet/standard/base-types/regular-expressions.

## Create custom DLP policies

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to create a custom DLP policy without any existing rules

1. In the EAC, navigate to **Compliance management** \> **Data loss prevention**. Any existing policies that you have configured are shown in a list.

2. Click the arrow that is beside the **Add** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif) icon, and select **New custom policy**.

   > [!IMPORTANT]
   > If you click **Add** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif) icon instead of the arrow, you will create a new policy based on a template. For more information about using templates, see [Create a DLP policy from a template](create-dlp-policy-from-template.md).

3. On the **New custom policy** page, complete the following fields:

   1. **Name**: Add a name that will distinguish this policy from others.

   2. **Description**: Add an optional description that summarizes this policy.

   3. **Choose a state**: Select the mode or state for this policy. The new policy is not fully enabled until you specify that it should be. The default mode for a policy is test without notifications.

4. Click **Save** to finish creating the new policy reference information. The policy is added to the list of all policies that you have configured, although there are not yet any rules or actions associated with this new custom policy.

5. Double-click the policy that you just created or select it and click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

6. On the **Edit DLP policy** page, click **Rules**.

   Click **Add** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif) to add a new blank rule. You can establish conditions using all the traditional mail flow rules in addition to the sensitive information types.

   In order to avoid confusion, supply a unique name for each part of your policy or rule when you have the option to provide your own character string. There are several options additional options available to you:

   1. Click the arrow that is beside the **Add** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif) icon to add a rule about sender notification or allowing overrides.

   2. To remove a rule, highlight the rule and click **Delete** ![Delete icon](../../media/ITPro_EAC_DeleteIcon.gif).

   3. Click **More options** ![More Options Icon](../../media/ITPro_EAC_MoreOptionsIcon.gif) to add additional conditions and actions for this rule including time-bound limits of enforcement or effects on other rules in this policy.

7. Click **Save** to finish modifying the policy and save your changes.

DLP policy templates are one type of feature Exchange Online that can help you design and apply a robust policy and compliance system for your messaging environment. For more information about compliance features, see [Security and compliance for Exchange Online](../../security-and-compliance/security-and-compliance.md).

## For more information

[Data loss prevention](data-loss-prevention.md)

[Mail flow rules (transport rules) in Exchange Online](../../security-and-compliance/mail-flow-rules/mail-flow-rules.md) Exchange Online

[Integrating sensitive information rules with mail flow rules](integrate-sensitive-information-rules.md)
