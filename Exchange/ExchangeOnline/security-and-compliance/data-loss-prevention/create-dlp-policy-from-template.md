---
localization_priority: Normal
description: 'In Microsoft Exchange, you can use data loss prevention (DLP) policy templates to help meet the messaging policy and compliance needs of your organization. These templates contain pre-built sets of rules that can help you manage message data that is associated with several common legal and regulatory requirements. To see a list of all the templates supplied by Microsoft, see DLP policy templates supplied in Exchange. Example DLP templates that are supplied can help you manage:'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 4432ef8b-6108-48d3-b2af-43ef5b40d2bc
ms.reviewer: 
f1.keywords:
- NOCSH
title: Create a DLP policy from a template
ms.collection:
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Create a DLP policy from a template

In Microsoft Exchange, you can use data loss prevention (DLP) policy templates to help meet the messaging policy and compliance needs of your organization. These templates contain pre-built sets of rules that can help you manage message data that is associated with several common legal and regulatory requirements. To see a list of all the templates supplied by Microsoft, see [DLP policy templates supplied in Exchange](dlp-policy-templates.md). Example DLP templates that are supplied can help you manage:

- Gramm-Leach-Bliley Act (GLBA) data

- Payment Card Industry Data Security Standard (PCI-DSS)

- United States Personally Identifiable Information (U.S. PII)

You can customize any of these DLP templates or use them as-is. DLP policy templates are built on top of mail flow rules (also known as transport rules) that include new conditions or predicates and actions. DLP policies support the full range of traditional mail flow rules, and you can add the additional rules after a DLP policy has been established. For more information about policy templates, see [What the DLP policy templates include](https://docs.microsoft.com/office365/securitycompliance/what-the-dlp-policy-templates-include). To learn more about mail flow rule capabilities, see [Mail flow rules (transport rules) in Exchange Online](../../security-and-compliance/mail-flow-rules/mail-flow-rules.md). Once you have started enforcing a policy, you can learn about how to observe the results by reviewing the Exchange Online: [View the reports for data loss prevention](https://docs.microsoft.com/microsoft-365/compliance/view-the-dlp-reports).

> [!CAUTION]
> You should enable your DLP policies in test mode before running them in your production environment. During such tests, it is recommended that you configure sample user mailboxes and send test messages that invoke your test policies in order to confirm the results.

## What do you need to know before you begin?

- Estimated time to complete: 30 minutes

- Configure both administrator and user accounts within your organization and validate basic mail flow.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Data loss prevention (DLP)" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to configure a DLP policy from a template

1. In the EAC, navigate to **Compliance management** \> **Data loss prevention**, and then click **Add** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif).

    > [!NOTE]
    > You can also select this action if you click the arrow next to the **Add** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif) icon and select **New DLP policy from template** from the drop down menu.

2. On the **Create a new DLP policy from a template** page, complete the following fields:

1. **Name**: Add a name that will distinguish this policy from others.

2. **Description**: Add an optional description that summarizes this policy.

3. **Choose a template**: Select the appropriate template to begin creating a new policy.

4. **More options**: Select the mode or state. The new policy is not fully enabled until you specify that it should be. The default mode for a policy is test without notifications.

5. Click **Save** to finish creating the policy.

> [!NOTE]
> In addition to the rules within a specific template, your organization may have additional expectations or company policies that apply to regulated data within your messaging environment. Exchange Server makes it easy for you to change the basic template in order to add actions so that your Exchange messaging environment complies with your own requirements.

You can modify policies by editing the rules within them once the policy has been saved in your Exchange Server environment. An example rule change might include making specific people exempt from a policy or sending a notice and blocking message delivery if a message is found to have sensitive content.

You have to navigate to the specific policy's rule set on the **Edit DLP policy** page and use the tools available on that page in order to change a DLP policy you have already created in Exchange Server.

Some policies allow the addition of rules that invoke RMS for messages. You must have RMS configured on the Exchange server before adding the actions to make use of these types of rules.

For any of the DLP policies, you can change the rules, actions, exceptions, enforcement time period or whether other rules within the policy are enforced and you can add your own custom conditions for each.

## For more information

[Data loss prevention](data-loss-prevention.md)
