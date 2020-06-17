---
title: 'Manage transport rules: Exchange 2013 Help'
TOCTitle: Manage transport rules
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: e7a81372-b6d7-4d1f-bc9e-a845a7facac2
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Manage transport rules in Exchange 2013

_**Applies to:** Exchange Server 2013_

You can use transport rules, also known as transport rules, to look for specific conditions on messages that pass through your organization and take action on them. This topic shows you how to [create](#create-a-transport-rule), [copy](#create-a-transport-rule), [adjust the order](#set-the-priority-of-a-transport-rule), [enable or disable](#enable-or-disable-a-transport-rule), [delete](#remove-a-transport-rule), or [import or export](#import-or-export-a-transport-rule-collection) rules.

> [!TIP]
> To make sure your rules work the way you expect, be sure to thoroughly test each rule and interactions between rules.

Interested in scenarios where these procedures are used? See the following topics:

- **Organization-wide Disclaimers, Signatures, Footers, or Headers**

- **Use transport rules to inspect message attachments in Exchange 2013**

- [Common attachment blocking scenarios for transport rules](common-attachment-blocking-scenarios-exchange-2013-help.md)

- [Use transport rules to route email based on a list of words, phrases, or patterns](use-rules-to-route-email-exchange-2013-help.md)

- [Common message approval scenarios](common-message-approval-scenarios-exchange-2013-help.md)

- [Best practices for configuring transport rules](best-practices-for-transport-rules-exchange-2013-help.md)

- **Use transport rules to aggressively filter bulk email messages**

- [Define rules to encrypt email messages](https://docs.microsoft.com/microsoft-365/compliance/define-mail-flow-rules-to-encrypt-email)

- **Create a Domain or User-Based Safe Sender or Blocked Sender List Using Transport Rules**

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Transport rules" entry in [Messaging policy and compliance permissions](messaging-policy-and-compliance-permissions-exchange-2013-help.md).

- When a rule is listed as **version 14**, this means that the rule is based on an Exchange Server 2010 transport rule format. All options are available for these rules.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Create a transport rule

You can create a transport rule by setting up a Data Loss Prevention (DLP) policy, creating a new rule, or by copying a rule. You can use the Exchange admin center (EAC) or the Exchange Management Shell.

> [!NOTE]
> After you create or modify a transport rule, it can take up to 30 minutes for the new or updated rule to be applied to email.

### Use a DLP policy to create transport rules

Each DLP policy is a collection of transport rules. After you create the DLP policy, you can fine-tune the rules using the procedures below.

1. Create a DLP policy. For instructions, see [DLP procedures](dlp-procedures-exchange-2013-help.md)).

2. Modify the transport rules created by the DLP policy. See [View or modify a transport rule](manage-transport-rules-exchange-2013-help.md#view-or-modify-a-transport-rule).

### Use the EAC to create a transport rule

The EAC allows you to create transport rules by using a template, copying an existing rule, or from scratch.

1. Go to **Mail flow** \> **Rules**.

2. Create the rule by using one of the following options:

   - To create a rule from a template, click **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif) and select a template.

   - To copy a rule, select the rule, and then select **Copy** ![Copy Icon](images/ITPro_EAC_CopyIcon.gif).

   - To create a new rule from scratch, **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif) and then select **Create a new rule**.

3. In the **New rule** dialog box, name the rule, and then select the conditions and actions for this rule:

   1. In **Apply this rule if...**, select the condition you want from the list of available conditions.

      - Some conditions require you to specify values. For example, if you select **The sender is...** condition, you must specify a sender address. If you're adding a word or phrase, note that trailing spaces are not allowed.

      - If the condition you want isn't listed, or if you need to add exceptions, select **More options**. Additional conditions and exceptions will be listed.

      - If you don't want to specify a condition, and want this rule to apply to every message in your organization, select **[Apply to all messages]** condition.

   2. In **Do the following...**, select the action you want the rule to take on messages matching the criteria from the list of available actions.

      - Some of the actions will require you to specify values. For example, if you select the **Forward the message for approval to...** condition, you will need to select a recipient in your organization.

      - If the condition you want isn't listed, select **More options**. Additional conditions will be listed.

   3. Specify how rule match data for this rule is displayed in the [Data Loss Prevention (DLP) reports](https://go.microsoft.com/fwlink/p/?LinkId=402768) and the [transport rule reports](https://go.microsoft.com/fwlink/p/?).

      Under **Audit this rule with severity level**, select a level to specify the severity level for this rule. Severity level is just a filter to make the reports easier to use. The severity level has no impact on the priority in which the rule is processed.

      > [!NOTE]
      > If you clear the **Audit this rule with severity level** checkbox, rule matches will not show up in the rule reports.

   4. Set the mode for the rule. You can use one of the two test modes to test the rule without impacting mail flow. In both test modes, when the conditions are met, an entry is added to the message trace.

      - **Enforce**: This turns on the rule and it starts processing messages immediately. All actions on the rule will be performed.

      - **Test with Policy Tips**: This turns on the rule, and any Policy Tip actions ( **Notify the sender with a Policy Tip**) will be sent, but no actions related to message delivery will be performed. Data Loss Prevention (DLP) is required in order to use this mode. To learn more, see [Policy Tips](policy-tips-exchange-2013-help.md).

     - **Test without Policy Tips**: Only the Generate incident report action will be enforced. No actions related to message delivery are performed.

4. If you are satisfied with the rule, go to step 5. If you want to add more conditions or actions, or if you want to specify exceptions or set additional properties, click **More options**. After you click **More options**, complete the following fields to create your rule:

   1. To add more conditions, click **Add condition**. If you have more than one condition, you can remove any one of them by clicking **Remove X** next to it. Note that there are a larger variety of conditions available once you click **More options**.

   2. To add more actions, click **Add action**. If you have more than one action, you can remove any one of them by clicking **Remove X** next to it. Note that there are a larger variety of actions available once you click **More options**.

   3. To specify exceptions, click **Add exception**, then select exceptions using the **Except if...** dropdown. You can remove any exceptions from the rule by clicking the **Remove X** next to it.

   4. If you want this rule to take effect after a certain date, click **Activate this rule on the following date:** and specify a date. Note that the rule will still be enabled prior to that date, but it won't be processed.

      Similarly, you can have the rule stop processing at a certain date. To do so, click **Deactivate this rule on the following date:** and specify a date. Note that the rule will remain enabled, but it won't be processed.

   5. You can choose to avoid applying additional rules once this rule processes a message. To do so, click **Stop processing more rules**. If you select this, and a message is processed by this rule, no subsequent rules are processed for that message.

   6. You can specify how the message should be handled if the rule processing can't be completed. By default, the rule will be ignored and the message will be processed regularly, but you can choose to resubmit the message for processing. To do so, check the **Defer the message if rule processing doesn't complete** check box.

   7. If your rule analyzes the sender address, it only examines the message headers by default. However, you can configure your rule to also examine the SMTP message envelope. To specify what's examined, click one of the following values for **Match sender address in message**:

      - **Header**: Only the message headers will be examined.

      - **Envelope**: Only the SMTP message envelope will be examined.

      - **Header or envelope**: Both the message headers and SMTP message envelope will be examined.

   8. You can add comments to this rule in the **Comments** box.

5. Click **Save** to complete creating the rule.

### Use the Exchange Management Shell to create a transport rule

This example uses the [New-TransportRule](https://docs.microsoft.com/powershell/module/exchange/new-transportrule) cmdlet to create a new transport rule that prepends " `External message to Sales DG:`" to messages sent from outside the organization to the Sales Department distribution group.

```powershell
New-TransportRule -Name "Mark messages from the Internet to Sales DG" -FromScope NotInOrganization -SentTo "Sales Department" -PrependSubject "External message to Sales DG:"
```

The rule parameters and action used in the above procedure are for illustration only. Review all the available transport rule conditions and actions to determine which ones meet your requirements.

### How do you know this worked?

To verify that you have successfully created a new transport rule, do the following:

- From the EAC, verify that the new transport rule you created is listed in the **Rules** list.

- From the Exchange Management Shell, verify that you created the new transport rule successfully by running the following command (the example below verifies the rule created in the Exchange Management Shell example above):

  ```powershell
  Get-TransportRule "Mark messages from the Internet to Sales DG"
  ```

## View or modify a transport rule

> [!NOTE]
> After you create or modify a transport rule, it can take up to 30 minutes for the new or updated rule to be applied to email.

### Use the EAC to view or modify a transport rule

1. From the EAC, go to **Mail flow** \> **Rules**.

2. When you select a rule in the list, the conditions, actions, exceptions and select properties of that rule are displayed in the details pane. To view all the properties of a specific rule, double click it. This opens the rule editor window, where you can make changes to the rule. For more information about rule properties, see [Use the EAC to create a transport rule](#use-the-eac-to-create-a-transport-rule) section, earlier in this topic.

### Use the Exchange Management Shell to view or modify a transport rule

The following example gives you a list of all rules configured in your organization:

```powershell
Get-TransportRule
```

To view the properties of a specific transport rule, you provide the name of that rule or its GUID. It is usually helpful to send the output to the **Format-List** cmdlet to format the properties. The following example returns all the properties of the transport rule named Sender is a member of Marketing:

```powershell
Get-TransportRule "Sender is a member of marketing" | Format-List
```

To modify the properties of an existing rule, use the [Set-TransportRule](https://docs.microsoft.com/powershell/module/exchange/set-transportrule) cmdlet. This cmdlet allows you to change any property, condition, action or exception associated with a rule. The following example adds an exception to the rule "Sender is a member of marketing" so that it won't apply to messages sent by the user Kelly Rollin:

```powershell
Set-TransportRule "Sender is a member of marketing" -ExceptIfFrom "Kelly Rollin"
```

### How do you know this worked?

To verify that you have successfully modified a transport rule, do the following:

- From the rules list in the EAC, click the rule you modified in the **Rules** list and view the details pane.

- From the Exchange Management Shell, verify that you modified the transport rule successfully by running the following command to list the properties you modified along with the name of the rule (the example below verifies the rule modified in the Exchange Management Shell example above):

  ```powershell
  Get-TransportRule "Sender is a member of marketing" | Format-List Name,ExceptIfFrom
  ```

## Transport rule properties

You can also use the **Set-TransportRule** cmdlet to modify existing transport rules in your organization. Below is a list properties not available in the EAC that you can change. For more information on using the **Set-TransportRule** cmdlet to make these changes see [Set-TransportRule](https://docs.microsoft.com/powershell/module/exchange/set-transportrule)

|**Condition Name in the EAC**|**Condition name in Exchange Management Shell**|**Properties**|**Description**|
|:-----|:-----|:-----|:-----|
|**Stop Processing Rules**| `StopRuleProcessing`| ` Not applicable `|Enables you to stop processing additional rules|
|**Header/Envelope matching**| `SenderAddressLocation`|Not applicable|Enables you to examine the SMTP message envelope to ensure the header and envelop match|
|**Audit severity **| `SetAuditSeverity`| `Not applicable`|Enables you to select a severity level for the audit|
|**Rule modes**| `Mode`| `Not applicable`|Enables you to set the mode for the rule|

## Set the priority of a transport rule

The rule at the top of the list is processed first. This rule has a **Priority** of 0.

### Use the EAC to set the priority of a rule

1. From the EAC, go to **Mail flow** \> **Rules**. This displays the rules in the order in which they are processed.

2. Select a rule, and use the arrows to move the rule up or down the list.

### Use the Exchange Management Shell to set the priority of a rule

The following example sets the priority of "Sender is a member of marketing" to 2:

```powershell
Set-TransportRule "Sender is a member of marketing" priority "2"
```

### How do you know this worked?

To verify that you have successfully modified a transport rule, do the following:

- From the rules list in the EAC, look at the order of the rules.

- From the Exchange Management Shell, verify the priority of the rules (the example below verifies the rule modified in the Exchange Management Shell example above):

  ```powershell
  Get-TransportRule * | Format-List Name,Priority
  ```

## Enable or disable a transport rule

Rules are enabled when you create them. You can disable a transport rule.

### Use the EAC to enable or disable a transport rule

1. From the EAC, go to **Mail flow** \> **Rules**.

2. To disable a rule, clear the check box next to its name.

3. To enable a disabled rule, select the check box next to its name.

### Use the Exchange Management Shell to enable or disable a transport rule

The following example disables the transport rule "Sender is a member of marketing":

```powershell
Disable-TransportRule "Sender is a member of marketing"
```

The following example enables the transport rule "Sender is a member of marketing":

```powershell
Enable-TransportRule "Sender is a member of marketing"
```

### How do you know this worked?

To verify that you have successfully enabled or disabled a transport rule, do the following:

- From the EAC, view the list of rules in the **Rules** list and check the status of the check box in the **ON** column.

- From the Exchange Management Shell, run the following command which will return a list of all rules in your organization along with their status:

  ```powershell
  Get-TransportRule | Format-Table Name,State
  ```

## Remove a transport rule

### Use the EAC to remove a transport rule

1. From the EAC, go to **Mail flow** \> **Rules**.

2. Select the rule you want to remove and then click **Delete** ![Delete icon](images/ITPro_EAC_DeleteIcon.gif).

### Use the Exchange Management Shell to remove a transport rule

The following example removes the transport rule "Sender is a member of marketing":

```powershell
Remove-TransportRule "Sender is a member of marketing"
```

### How do you know this worked?

To verify that you have successfully removed the transport rule, do the following:

- From the EAC, view the rules in the **Rules** list and verify that the rule you removed is no longer shown.

- From the Exchange Management Shell, run the following command and verify that the rule you remove is no longer listed:

  ```powershell
  Get-TransportRule
  ```

## Import or export a transport rule collection

You must use the Exchange Management Shell to import or export a transport rule collection. For information about how to import a transport rule collection from an XML file, see [Import-TransportRuleCollection](https://docs.microsoft.com/powershell/module/exchange/import-transportrulecollection). For information about how to export a transport rule collection to an XML file, see [Export-TransportRuleCollection](https://docs.microsoft.com/powershell/module/exchange/export-transportrulecollection).

## Need more help?

[Transport rules in Exchange 2013](mail-flow-rules-transport-rules-in-exchange-2013-exchange-2013-help.md)

[Transport rule conditions and exceptions (predicates) in Exchange 2013](mail-flow-rule-conditions-and-exceptions-predicates-in-exchange-2013-exchange-2013-help.md)

[Transport rule actions in Exchange 2013](mail-flow-rule-actions-in-exchange-2013-exchange-2013-help.md)
