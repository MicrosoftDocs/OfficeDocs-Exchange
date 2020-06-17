---
localization_priority: Normal
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 708c71d5-7d6a-40b8-966b-eed82bc0d186
ms.reviewer: 
manager: serdars
description: Admins can learn how to use a screen reader to create mail flow rules (also known as transport rules) in the Exchange admin center (EAC) in Exchange Online.
title: Use a screen reader to configure mail flow rules in the Exchange admin center in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
f1.keywords:
- CSH
ms.custom: A11y_UseSR
ms.service: exchange-online

---

# Use a screen reader to configure mail flow rules in the Exchange admin center in Exchange Online

Using a screen reader and keyboard shortcuts, you can create mail flow rules (also known as transport rules) in Exchange Online in the Exchange admin center (EAC) to look for specific conditions in messages that pass through your organization and take action on them. The main difference between mail flow rules and Inbox rules you would set up in an email client application (such as Outlook) is that mail flow rules take action on messages while they're in transit as opposed to after the message is delivered. Mail flow rules also contain a richer set of conditions, exceptions, and actions, which provides you with the flexibility to implement many types of messaging policies.

 **Note**: To learn more about mail flow rules, see [Mail flow rules (transport rules) in Exchange Online](../security-and-compliance/mail-flow-rules/mail-flow-rules.md).

## Get started

Navigate with Internet Explorer and keyboard shortcuts, and make sure that you have the appropriate Office 365 or Microsoft 365 subscription plan and admin role to perform this task. Then, open the EAC and get started.

### Use your browser and keyboard to navigate in the EAC

Exchange Online, which includes the EAC, is a web-based application, so the keyboard shortcuts and navigation may be different from those in Exchange 2016. [Accessibility in the Exchange admin center](accessibility-in-exchange-admin-center.md).

For best results when working in the EAC in Exchange Online, use Internet Explorer as your browser. [Learn more about Internet Explorer keyboard shortcuts](https://support.microsoft.com/help/17456/).

Many tasks in the EAC require the use of pop-up windows so, in your browser, be sure to [enable pop-up windows for Office 365](https://support.microsoft.com/help/17479).

### Confirm your Office 365 or Microsoft 365 subscription plan

Exchange Online is included in several different subscription plans, but capabilities may differ by plan. If your EAC doesn't include a function described in this article, your plan might not include it.

For more information about the Exchange Online capabilities in your subscription plan, go to [What Office 365business product or license do I have?](https://support.office.com/article/f8ab5e25-bf3f-4a47-b264-174b1ee925fd) and [Exchange Online Service Description](https://docs.microsoft.com/office365/servicedescriptions/exchange-online-service-description/exchange-online-service-description).

### Open the EAC, and confirm your admin role

To complete the tasks covered in this topic, [Use a screen reader to open the Exchange admin center](use-screen-reader-to-open-exchange-admin-center.md) and check that your global administrator has assigned you to the Organization Management and Records Management admin role groups. Learn how to [Use a screen reader to identify your admin role in the Exchange admin center](use-screen-reader-to-identify-admin-role-in-exchange-admin-center.md).

## Create a mail flow rule

1. In the EAC, to move the focus to the first link in the navigation pane (**Dashboard**) press Ctrl+F6 *twice*. You hear "Dashboard, Primary navigation link..

2. To move the focus to the **mail flow** link in the navigation pane, press the Tab key until you hear "Mail flow, Primary navigation link." Press Enter.

3. To move the focus to the mail flow settings in the content area of the page, the first of which is the **rules** link, press Ctrl+F6. You hear "Rules, Secondary navigation link..

4. To create a new rule, move the focus to the **New** button by pressing the Tab key until you hear "New button." Press Enter. You hear "Menu." To select the **Create a new rule** option from the list of options that opens for the button, press the Down Arrow key. You hear "Create a new rule." Press Enter.

5. As the focus moves to the **Name** text box in the **new rule** pop-up window, you hear "New rule, Name, Edit." Type the name of the new rule. To move to the next option in the window, press the Tab key.

6. As the focus moves to the **Apply this rule if** drop-down box, you hear "Apply this rule if, Combo box." Press the Down Arrow or Up Arrow key until you hear the condition you want to select. Press Enter. As the focus moves to the first user interface (UI) element in the pop-up window that opens for the selected condition, you hear the name of the pop-up window followed by the name of the first UI element in the window. The following table gives you an overview of the UI elements in each condition's pop-up window.
.
   |**Condition**|**UI elements in the condition's pop-up window**|
   |:-----|:-----|
   |• The sender is <br/>• The recipient is <br/>• The sender is a member of <br/>• The recipient is a member of|• **Search**, **Refresh**, and **More** buttons. <br/>• **Display Name** and **Email Address** column headers. <br/>• List of names and email addresses. <br/>• **Add** button and text box that includes the selected names. <br/>• **Check names** button and text box in which you type the name you want to check. <br/>• **OK** and **Cancel** buttons.|
   |• The sender is located <br/>• The recipient is located|• Drop-down box that opens a list of locations. <br/>• OK and Cancel buttons.|
   |• The subject or body includes <br/>• The sender address includes <br/>• The recipient address includes <br/>• Any attachment's content includes|• Edit and Remove buttons. <br/>• Text box in which you type words, and an **Add** button to add each entry. <br/>• List of entries. <br/>• **OK** and **Cancel** buttons.|
   |[Apply to all messages]|No pop-up window opens|

   > [!TIP]
   > To move the focus to each setting that's listed in a pop-up window, press the Tab key. As you select each setting, you hear information about it. To open drop-down box lists, press Spacebar. To move between and select options in drop-down box lists, press the Down Arrow and Up Arrow keys. To choose an option, press Enter. You can also use the Spacebar to select or clear the selection for check boxes.

7. After you've accepted your condition settings in the appropriate pop-up window, move to the next option in the **new rule** pop-up window by pressing the Tab key.

8. As the focus moves to the **Do the following** drop-down box, you hear "Do the following, Combo box." Press the Down Arrow or Up Arrow key until you hear the action you want to select. Press Enter. As the focus moves to the first UI element in the pop-up window that opens for the selected action, you hear the name of the pop-up window followed by the name of the first UI element in the window. The following table gives you an overview of the UI elements in each action's pop-up window.

   |**Action**|**UI elements in the pop-up window**|
   |:-----|:-----|
   |• Forward the message for approval to <br/>• Redirect the message to <br/>• Bcc the message to|• **Search**, **Refresh**, and **More** buttons. <br/>• **Display Name** and **Email Address** column headers. <br/>• List of names and email addresses. <br/>• **Add** button and text box that includes the selected names. <br/>• **Check names** button and text box in which you type the name you want to check. <br/>• **OK** and **Cancel** buttons.|
   |Reject the message with the explanation|• Text box in which you type the explanation OK <br/>• **OK** and **Cancel** buttons.|
   |Delete the message without notifying anyone|No pop-up window opens|
   |Append the disclaimer|No pop-up window opens, but an **Enter text** link and a **Select one** link are inserted in the window after the drop-down box. <br/>• If you select the **Enter text** link, a pop-up window opens that includes a text box in which you type the disclaimer, and the **OK** and **Cancel** buttons. <br/>• If you select the **Select one** link, a pop-up window opens that includes a drop-down box that opens a list of fallback actions in case the disclaimer can't be inserted, and the **OK** and **Cancel** buttons.|

9. After you've accepted your action settings in the appropriate pop-up window, move to the next option in the **new rule** pop-up window by pressing the Tab key.

10. As the focus moves to the **Audit this rule with severity level** check box, you hear "Checked" or "Unchecked" depending on whether the box is selected or not, followed by "Audit this rule with severity level, Check box." To select or clear the selection for the check box, press Spacebar. You hear "Checked" or "Unchecked." Do either of the following two actions.

   - If you selected the **Audit this rule with severity level** check box, when you press the Tab key, the focus moves to a drop-down box that lists severity levels ( **Low**, **Medium**, or **High** ). To move between severity levels in the list, press the Up Arrow or Down Arrow key. You hear the name of each severity level. To select a severity level, press Enter. To move to the next option in the window, press the Tab key.

   - If you didn't select the **Audit this rule with severity level** check box, to move to the next available option in the window, press the Tab key.

11. As the focus moves to the first of three available modes for the rule, you hear the name of the first mode ( **Enforce** ) followed by "Radio button." Do any of the following three actions.

   - The **Enforce** mode is selected by default. To move to and select the next mode, press the Down Arrow key. After you've selected the mode you want, to move to the next area of options in the window, press the Tab key.

   - To select the **Test with Policy Tips** mode, press the Down Arrow key. You hear "Test with Policy Tips" followed by "Radio button." To move to and select the next mode, press the Down Arrow key. After you've selected the mode you want, to move to the next area of options in the window, press the Tab key.

   - To select the **Test without Policy Tips** mode, press the Down Arrow key. You hear "Test without Policy Tips" followed by "Radio button." To move to and select the next mode, press the Down Arrow key. After you've selected the mode you want, to move to the next area of options in the window, press the Tab key.

12. As the focus moves to the **More options** link, you hear "More options link." If you want to add more options for the rule, press Enter. The following nine UI elements are added to the window.

   - After the **Apply this rule if** drop-down box, an **add condition** button is added.

   - After the **Do the following** drop-down box, an **add action** button is added.

   - After the **add action** button, an **add exception** button is added.

   - After the options for the modes for the rule, the following UI elements are added:

   - **Activate this rule on the following date** check box, followed by a date drop-down box and a time drop-down bo.

   - **Deactivate this rule on the following date** check box, followed by a date drop-down box and a time drop-down bo.

   - **Stop processing more rules** check bo.

   - **Defer the message if rule processing doesn't complete** check bo.

   - **Match sender address in message** drop-down box that includes **Header**, **Envelope**, and **Header or Envelope** option.

   - **Comment** text bo.

13. To save the new rule, move the focus to the **Save** button by pressing the Tab key until you hear "Save button." Press Enter.
  .
14. As the focus moves back to the **New** button on the **rules** content area of the page, you hear "Rules, New button." The new rule is turned on by default.

     > [!TIP]
     > To turn off a new rule, press the Tab key to tab through the elements of the **rules** content area of the page, use the Up Arrow and Down Arrow keys to select a rule, and then press Spacebar. To hear the settings for a selected rule, press the Tab key until the focus moves to the details pane for the selected rule, and you hear the details for the rule.
