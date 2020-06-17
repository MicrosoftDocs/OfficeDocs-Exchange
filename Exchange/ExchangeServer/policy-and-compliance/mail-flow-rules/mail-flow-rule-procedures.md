---
localization_priority: Normal
description: 'Summary: Learn how to create, view, modify, delete, import, and export mail flow rules (transport rules) in Exchange 2016 and Exchange 2019'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: e7a81372-b6d7-4d1f-bc9e-a845a7facac2
ms.reviewer:
title: Procedures for mail flow rules in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Procedures for mail flow rules in Exchange Server

Mail flow rules (also known as transport rules) identify and take action on messages that flow through your Exchange organization. For more information about mail flow rules, see [Mail flow rules in Exchange Server](mail-flow-rules.md).

On Mailbox servers, you can manage mail flow rules in the Exchange admin center (EAC) and in the Exchange Management Shell. On Edge Transport servers, you can only use the Exchange Management Shell.

> [!TIP]
> Verify that your rules work the way you expect. Be sure to thoroughly test each rule and the interactions between rules.

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes.

- For more information about the EAC, see [Exchange admin center in Exchange Server](../../architecture/client-access/exchange-admin-center.md). To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mail flow rules" entry in [Messaging policy and compliance permissions in Exchange Server](../../permissions/feature-permissions/policy-and-compliance-permissions.md) (Exchange Server), or in [Feature Permissions in Exchange Online](https://docs.microsoft.com/exchange/permissions-exo/feature-permissions).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Create mail flow rules

- Creating mail flow rules is mostly about the scenarios that you want to fulfill. For examples, see the following topics:

  - [Use mail flow rules to inspect message attachments](https://docs.microsoft.com/exchange/use-transport-rules-to-inspect-message-attachments-exchange-2013-help)

  - [Organization-wide disclaimers, signatures, footers, or headers in Exchange Server](signatures.md)

  - [Manage message approval](https://docs.microsoft.com/Exchange/manage-message-approval-exchange-2013-help)

- Data Loss Prevention (DLP) policies are collections of mail flow rules. To create DLP policies, see [Exchange Server DLP Procedures](https://docs.microsoft.com/exchange/dlp-procedures-exchange-2013-help).

### Use the EAC to create mail flow rules

The EAC allows you to create mail flow rules by using a template (a filtered list of conditions and actions), by copying an existing rule, or by creating a rule from scratch.

1. In the EAC, go to **Mail flow** \> **Rules**, and then select one of the following options:

   - To create a rule from a template, click **Add** (![Add icon](../../media/ITPro_EAC_AddIcon.png)) and select a template (a value other than **Create new rule**).

   - To copy a rule, select the rule, and then select **Copy** (![Copy icon](../../media/ITPro_EAC_CopyIcon.png)). Note that the option to copy a rule is only available in the EAC.

   - To create a new rule from scratch, **Add** (![Add icon](../../media/ITPro_EAC_AddIcon.png)) and then select **Create a new rule**.

2. In the **New rule** page that opens, configure the following settings:

   - **Name**: Enter a unique, descriptive name for the rule.

   - **Apply this rule if**: Select a condition for the rule. If you want the rule to apply to all messages, select **[Apply to all messages]**. For an explanation of the available conditions, see [Mail flow rule conditions and exceptions (predicates) in Exchange Server](conditions-and-exceptions.md).

   - **Do the following**: Select an action for the rule. The action is applied to messages that match the conditions. For an explanation of the available conditions, see [Mail flow rule actions in Exchange Server](actions.md).

   Optional properties:

   - **Audit this rule with severity level**: For DLP policies, this setting specifies how rule match data is displayed in the DLP policy detection reports. For more information, [View DLP policy detection reports](https://docs.microsoft.com/exchange/view-dlp-policy-detection-reports-exchange-2013-help). If you clear the check box, or select the value **Not specified**, rule matches won't appear in the rule reports.

   - **Choose a mode for this rule**: You can use one of the two test modes to test the rule without impacting mail flow. In both test modes, when the conditions are met, an entry is added to the message tracking log. Select one of the following values:

   - **Enforce**: This turns on the rule and it starts processing messages immediately. All actions on the rule will be performed. This is the default value.

   - **Test with Policy Tips**: This turns on the rule, and any Policy Tip actions (**Notify the sender with a Policy Tip**) will be sent, but no actions related to message delivery will be performed. DLP is required to use this mode. To learn more, see [Policy Tips](https://docs.microsoft.com/Exchange/policy-tips-exchange-2013-help).

   - **Test without Policy Tips**: For DLP policies, only the **Generate incident report and send it to** action will be enforced. No actions related to message delivery are performed.

3. You can create the rule by clicking **Save**, or you can click **More options** to configure the following additional settings:

   - To add more conditions, click **Add condition**. If you have more than one condition, you can remove a condition by clicking **Remove X**. Note that there are more conditions available after you click **More options**.

   - To add more actions, click **Add action**. If you have more than one action, you can remove an action by clicking **Remove X**. Note that there are more actions available after you click **More options**.

   - To add exceptions for the rule, click **Add exception**, and then select an exception by using the **Except if** drop down. You can remove an exception by clicking **Remove X**.

   - **Activate this rule on the following date**: Specify the start date if you want the rule to take effect after a certain date. Note that the rule will still be enabled prior to that date, but it won't be processed.

   - **Deactivate this rule on the following date**: Specify the end date if you want the rule to stop processing messages on a certain date. Note that the rule will still be enabled after that date, but it won't be processed.

   - **Stop processing more rules**: Select this check box to avoid applying additional rules after this rule processes a message.

   - **Defer the message if rule processing doesn't complete**: Select this check box to resubmit the message for processing. By default, the rule will be ignored, and delivery of the message will continue as normal.

   - **Match sender address in message**: For conditions and exceptions that examine the sender's address, you can specify where the rule looks for the sender's address: in the message header (default), the message envelope, or the header and envelope. For more information, see [Senders](conditions-and-exceptions.md#senders).

   - **Comments**: Specify a descriptive comment for the rule.

   When you're finished, click **Save**.

### Use the Exchange Management Shell to create mail flow rules

There are two settings that you can configure on new mail flow rules in the Exchange Management Shell that aren't available in the EAC (until after you create the rule):

- Create the new rule as disabled (_Enabled_ `$false`)

- Set the priority of the rule (_Priority_ _\<Number\>_).

To create mail flow rules in the Exchange Management Shell, use the following syntax:

```PowerShell
New-TransportRule -Name <RuleName> [<Conditions>] [<Exceptions>] <Actions> [<Properties>]
```

This example creates a new rule with the following settings:

- **Name**: Mark messages from the Internet to Sales DG.

- **Conditions**

  - Messages from external senders.

    And

  - Messages sent to the distribution group named Sales Department.

- **Action**: Prepend the message's **Subject** field with the value `"External message to Sales DG: "`. The trailing colon and space help to distinguish the added text from the original value.

```PowerShell
New-TransportRule -Name "Mark messages from the Internet to Sales DG" -FromScope NotInOrganization -SentTo "Sales Department" -PrependSubject "External message to Sales DG: "
```

For detailed syntax and parameter information, see [New-TransportRule](https://docs.microsoft.com/powershell/module/exchange/new-transportrule).

 **Note**: The conditions and actions in the example are for illustrative purposes only. Review the available mail flow rule conditions, exceptions, and actions to determine which ones meet your requirements.

### How do you know this worked?

To verify that you've successfully created a mail flow rule, use either of the following procedures:

- In the EAC, go to **Mail flow** \> **Rules**, and verify that the rule you created is in the list.

- In the Exchange Management Shell, use either of the following procedures:

  - Run the following command to see the new rule in the list of rules:

  ```PowerShell
  Get-TransportRule
  ```

  - Replace _\<RuleName\>_ with the name of the rule, and run the following command to see the details of the rule:

  ```PowerShell
  Get-TransportRule -Identity "<RuleName>" | Format-List
  ```

## View mail flow rules

Mail flow rules that you create on a Mailbox server are stored in Active Directory, so when you view the rules on a Mailbox server, you see all rules in your organization. When you use the Exchange Management Shell to view mail flow rules on an Edge Transport server, you see the rules that are stored on the local server.

### Use the EAC to view mail flow rules

1. In the EAC, go to **Mail flow** \> **Rules**.

2. When you select a rule, information about the rule is displayed in the details pane. To see more information about the rule, click **Edit** (![Edit icon](../../media/ITPro_EAC_EditIcon.png)).

   ![In the EAC, go to Mail flow \> Rules and select a rule](../../media/37502067-8f3f-49d1-aaea-91c7f3eb8e8a.png)

   In the EAC, the **Version** property is only visible in the details pane. This property indicates the compatibility of the rule with previous versions of Exchange (14.*n*.*n*.*n* is Exchange 2010, 15.0.*n*.*n* is Exchange 2013).

### Use the Exchange Management Shell to view mail flow rules

To return a summary list of all mail flow rules, run the following command:

```PowerShell
Get-TransportRule
```

To return detailed information about a specific rule, use the following syntax:

```PowerShell
Get-TransportRule -Identity "<RuleName>" | Format-List [<Specific properties to view>]
```

This example returns all the property values for the rule named "Sender is a member of marketing".

```PowerShell
Get-TransportRule -Identity "Sender is a member of marketing" | Format-List
```

This example returns only the specified properties for the same rule.

```PowerShell
Get-TransportRule -Identity "Sender is a member of marketing" | Format-List Name,State,Mode,Priority,Comments,Conditions,Exceptions,RuleVersion
```

For detailed syntax and parameter information, see [Get-TransportRule](https://docs.microsoft.com/powershell/module/exchange/get-transportrule).

### Use the Exchange Management Shell to view the available conditions and exceptions (predicates) for mail flow rules

The conditions and exceptions in mail flow rules are collectively known as *predicates* because for every condition, there's a corresponding exception that uses the exact same settings and syntax. The only difference is: conditions specify messages to include, while exceptions specify messages to exclude. You can only view the list of conditions and exceptions in the Exchange Management Shell.

To view the conditions and exceptions that are available in mail flow rules, run the following command:

```PowerShell
Get-TransportRulePredicate
```

For detailed syntax and parameter information, see [Get-TransportRulePredicate](https://docs.microsoft.com/powershell/module/exchange/get-transportrulepredicate).

 **Notes**:

- Exceptions aren't distinguished from conditions.

- The predicates that are available on Edge Transport servers are a small subset of those available on Mailbox servers. For more information, see [Mail flow rule conditions and exceptions (predicates) in Exchange Server](conditions-and-exceptions.md).

- Some of the predicate names are different than the corresponding condition and exception parameter names on the **New-TransportRule** and **Set-TransportRule** cmdlets. And, some predicates require multiple parameters.

### Use the Exchange Management Shell to view the available actions for mail flow rules

You can only view the list of actions in the Exchange Management Shell.

To view the actions that are available in mail flow rules, run the following command:

```PowerShell
Get-TransportRuleAction
```

For detailed syntax and parameter information, see [Get-TransportRuleAction](https://docs.microsoft.com/powershell/module/exchange/get-transportruleaction).

 **Notes**:

- A small subset of actions that are available on Mailbox servers are also available on Edge Transport servers, but some actions are only available on Edge Transport servers. For more information, see [Mail flow rule actions in Exchange Server](actions.md).

- Some of the action names are different than the corresponding action parameter names on the **New-TransportRule** and **Set-TransportRule** cmdlets. And, some actions require multiple parameters.

## Modify mail flow rules

### Use the EAC to modify mail flow rules

No additional settings are available when you modify a mail flow rule in the EAC. They're the same settings that were available when you created the rule.

1. In the EAC, go to **Mail flow** \> **Rules**.

2. Select the rule, and then click **Edit** (![Edit icon](../../media/ITPro_EAC_EditIcon.png)). Note that the properties of the rule are fully expanded (there's no **More options** link available). For more information about the rule properties, see the [Use the EAC to create mail flow rules](#use-the-eac-to-create-mail-flow-rules) section in this topic.

### Use the Exchange Management Shell to modify mail flow rules

When you modify a mail flow rule in the Exchange Management Shell, you can't disable or enable the rule (there's no _Enabled_ parameter on the **Set-TransportRule** cmdlet). Instead, you use the **Disable-TransportRule** and **Enable-TransportRule** cmdlets as describe later in this topic.

To modify a mail flow rule in the Exchange Management Shell, use the following syntax:

```PowerShell
Set-MailFlowRule -Identity "<RuleName>" [<Conditions>] [<Exceptions>] [<Actions>] [<Properties>]
```

This example adds an exception to the rule named "Sender is a member of marketing" so that it won't apply to messages that are sent by the user named Kelly Rollin.

```PowerShell
Set-TransportRule -Identity "Sender is a member of marketing" -ExceptIfFrom "Kelly Rollin"
```

For detailed syntax and parameter information, see [Set-TransportRule](https://docs.microsoft.com/powershell/module/exchange/set-transportrule).

### How do you know this worked?

To verify that you have successfully modified a mail flow rule, use either of the following procedures:

- In the EAC, go to **Mail flow** \> **Rules**, select the rule, and view the information in details pane. To see more settings, click **Edit** (![Edit icon](../../media/ITPro_EAC_EditIcon.png)).

- In the Exchange Management Shell, replace _\<RuleName\>_ with the name of the rule, and run the following command:

  ```PowerShell
  Get-TransportRule -Identity "<RuleName>" | Format-List
  ```

## Set the priority of mail flow rules

By default, mail flow rules are given a priority that's based on the order they were created in (newer rules are lower priority than older rules). A lower priority number indicates a higher priority for the rule, and rules are processed in priority order (higher priority rules are processed before lower priority rules). No two rules can have the same priority.

 **Notes**:

- You can prevent a message from being acted on by subsequent lower priority rules by including the **Stop processing more rules** (_StopRuleProcessing_ `$true`) action in the rule.

- In the EAC, you can only change the priority of the rule after you create it. In the Exchange Management Shell, you can override the default priority when you create the rule (which can affect the priority of existing rules).

### Use the EAC to set the priority of mail flow rules

In the EAC, rules are processed in the order that they're displayed (the first rule has the **Priority** value 0). To change the priority of a rule, move the rule up or down in the list (you can't directly modify the **Priority** number in the EAC).

1. In the EAC, go to **Mail flow** \> **Rules**.

2. Select a rule, and then click **Move up** (![Up Arrow Icon](../../media/ITPro_EAC_UpArrowIcon.png)) or **Move down** (![Down Arrow Icon](../../media/ITPro_EAC_DownArrowIcon.png)) to move the rule up or down in the list.

### Use the Exchange Management Shell to set the priority of mail flow rules

The highest priority value you can set on a rule is 0. The lowest value you can set depends on the number of rules. For example, if you have five rules, you can use the priority values 0 through 4. Changing the priority of an existing rule can have a cascading effect on other rules. For example, if you have five rules (priorities 0 through 4), and you change the priority of a rule to 2, the existing rule with priority 2 is changed to priority 3, and the rule with priority 3 is changed to priority 4.

To set the priority of a rule in the Exchange Management Shell, use the following syntax:

```PowerShell
Set-TransportRule -Identity "<RuleName>" -Priority <Number>
```

This example sets the priority of the rule named "Sender is a member of marketing" to 2. All existing rules that have a priority less than or equal to 2 are decreased by 1 (their priority numbers are increased by 1).

```PowerShell
Set-TransportRule -Identity "Sender is a member of marketing" -Priority 2
```

 **Note**: To set the priority of a new rule when you create it, use the _Priority_ parameter on the **New-TransportRule** cmdlet.

### How do you know this worked?

To verify that you have successfully modified the priority of a mail flow rule, use either of the following procedures:

- In the EAC, go to **Mail flow** \> **Rules**, and verify the **Priority** value of the rule in the list.

- In the Exchange Management Shell, use either of the following procedures:

  - Run the following command to see the list of rules and their **Priority** values:

  ```PowerShell
  Get-TransportRule
  ```

  - Replace _\<RuleName\>_ with the name of the rule, and run the following command:

  ```PowerShell
  Get-TransportRule -Identity "<RuleName>" | Format-List Name,Priority
  ```

## Enable or disable mail flow rules

Disabling a rule prevents the rule from acting on messages, but allows you to preserve the settings of the rule.

By default, mail flow rules are enabled when you create them in the EAC or the Exchange Management Shell, but you can use the Exchange Management Shell to create a disabled rule (use the _Enabled_ parameter with the value `$false`).

### Use the EAC to enable or disable mail flow rules

1. In the EAC, go to **Mail flow** \> **Rules**.

2. Select the rule from the list, and then configure one of the following settings:

   - **Disable the rule**: Clear the check box in the **On** column.

   - **Enable the rule**: Select the check box in the **On** column.

### Use the Exchange Management Shell to enable or disable mail flow rules

To enable or disable a mail flow rule in the Exchange Management Shell, use the following syntax:

```PowerShell
<Enable-TransportRule | Disable-TransportRule> -Identity "<RuleName>"
```

This example disables the mail flow rule named "Sender is a member of marketing".

```PowerShell
Disable-TransportRule "Sender is a member of marketing"
```

This example enables the mail flow rule named "Sender is a member of marketing".

```PowerShell
Enable-TransportRule "Sender is a member of marketing"
```

For detailed syntax and parameter information, see [Enable-TransportRule](https://docs.microsoft.com/powershell/module/exchange/enable-transportrule) and [Disable-TransportRule](https://docs.microsoft.com/powershell/module/exchange/disable-transportrule).

### How do you know this worked?

To verify that you have successfully enabled or disabled a mail flow rule, use either of the following procedures:

- In the EAC, go to **Mail flow** \> **Rules**, and in the list of rules verify the status of the check box in the **On** column.

- In the Exchange Management Shell, use either of the following procedures:

  - Run the following command to see the list of rules and their **State** values:

  ```PowerShell
  Get-TransportRule
  ```

  - Replace _\<RuleName\>_ with the name of the rule, and run the following command:

  ```PowerShell
  Get-TransportRule -Identity "<RuleName>" | Format-List Name,State
  ```

## Remove mail flow rules

### Use the EAC to remove mail flow rules

1. From the EAC, go to **Mail flow** \> **Rules**.

2. Select the rule you want to remove from the list, and then click **Delete** (![Delete icon](../../media/ITPro_EAC_DeleteIcon.png)).

### Use the Exchange Management Shell to remove mail flow rules

To remove mail flow rules in the Exchange Management Shell, use the following syntax:

```PowerShell
Remove-TransportRule -Identity "<RuleName>"
```

This example removes the mail flow rule named "Sender is a member of marketing":

```PowerShell
Remove-TransportRule -Identity "Sender is a member of marketing"
```

For detailed syntax and parameter information, see [Remove-TransportRule](https://docs.microsoft.com/powershell/module/exchange/remove-transportrule).

### How do you know this worked?

To verify that you have successfully removed a mail flow rule, use either of the following procedures:

- In the EAC, go to **Mail flow** \> **Rules**, and verify that the rule you removed is no longer in the list.

- In the Exchange Management Shell, run the following command to verify that the rule you removed is no longer listed:

  ```PowerShell
  Get-TransportRule
  ```

## Import or export mail flow rule collections

You can import a mail flow rule collection that you've previously exported as a backup, or import rules that you've exported from a previous version of Exchange.

 **Notes**:

- You can't import or export mail flow rule collections in the EAC. You can only use the Exchange Management Shell.

- You can't import a mail flow rule collection into Exchange 2010 if that rule collection was exported from Exchange 2013 or later.

### Use the Exchange Management Shell to export a mail flow rule collection

1. Run the following command:

   ```PowerShell
   $File = Export-TransportRuleCollection
   ```

2. Use the following syntax:

   ```PowerShell
   Set-Content -Path "<OutputFile>" -Value $file.FileData -Encoding Byte
   ```

   For example, to save the exported mail flow rule collection to the file C:\My Documents\Exported Rules.xml, run the following command:

   ```PowerShell
   Set-Content -Path "C:\My Documents\Exported Rules.xml" -Value $file.FileData -Encoding Byte
   ```

For detailed syntax and parameter information, see [Export-TransportRuleCollection](https://docs.microsoft.com/powershell/module/exchange/export-transportrulecollection).

### Use the Exchange Management Shell to import a mail flow rule collection

1. Use the following syntax:

   ```PowerShell
   [Byte[]]$Data = Get-Content -Path "<OutputFile>" -Encoding Byte -ReadCount 0
   ```

   For example, to import the mail flow rule collection from C:\My Documents\Exported Rules.xml, run the following command:

   ```PowerShell
   Byte[]]$Data = Get-Content -Path "C:\My Documents\Exported Rules.xml" -Encoding Byte -ReadCount 0
   ```

2. Run the following command:

   ```PowerShell
   Import-TransportRuleCollection -FileData $Data
   ```

For detailed syntax and parameter information, see [Import-TransportRuleCollection](https://docs.microsoft.com/powershell/module/exchange/import-transportrulecollection).

## Need more help?

- Resources for Exchange Server:

  - [Mail flow rules in Exchange Server](mail-flow-rules.md)

  - [Mail flow rule conditions and exceptions (predicates) in Exchange Server](conditions-and-exceptions.md)

  - [Mail flow rule actions in Exchange Server](actions.md)
