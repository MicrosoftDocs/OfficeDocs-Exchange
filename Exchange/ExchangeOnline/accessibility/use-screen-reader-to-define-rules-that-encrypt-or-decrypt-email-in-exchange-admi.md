---
localization_priority: Normal
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 3d8f2b49-c27e-40c9-808b-4bc48f3cdb07
ms.reviewer: 
manager: serdars
description: Admins can learn how to use a screen reader to create mail flow rules (also known as transport rules) in the Exchange admin center (EAC) that use Office 365 Message Encryption (OME) to encrypt and decrypt messages in Exchange Online.
title: Use a screen reader to define rules that encrypt or decrypt email messages in the Exchange admin center in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
f1.keywords:
- CSH
ms.custom: A11y_UseSR
ms.service: exchange-online

---

# Use a screen reader to define rules that encrypt or decrypt email messages in the Exchange admin center in Exchange Online

In the Exchange admin center (EAC) in Exchange Online, you can create mail flow rules (also known as transport rules) to enable or disable Office 365 Message Encryption. This lets you encrypt outgoing email messages and remove encryption from encrypted messages coming from inside your organization or from replies to encrypted messages sent from your organization.

 **Note**: To learn more about message encryption, go to [Encryption](https://docs.microsoft.com/microsoft-365/compliance/encryption). Your organization must have [Set up new Office 365 Message Encryption capabilities](https://docs.microsoft.com/Office365/securitycompliance/set-up-new-message-encryption-capabilities) to complete the tasks in this topic.

## Get started

Navigate with Internet Explorer and keyboard shortcuts, and make sure that you have the appropriate Office 365 or Microsoft 365 subscription plan and admin role to perform this task. Then, open the EAC and get started.

### Use your browser and keyboard to navigate in the EAC

Exchange Online, which includes the EAC, is a web-based application, so the keyboard shortcuts and navigation may be different from those in Exchange 2016. [Accessibility in the Exchange admin center](accessibility-in-exchange-admin-center.md).

For best results when working in the EAC in Exchange Online, use Internet Explorer as your browser. [Learn more about Internet Explorer keyboard shortcuts](https://support.microsoft.com/help/17456/).

Many tasks in the EAC require the use of pop-up windows so, in your browser, be sure to [enable pop-up windows for Office 365](https://support.microsoft.com/help/17479).

### Confirm your Office 365 or Microsoft 365 subscription plan

Exchange Online is included in several different subscription plans, but capabilities may differ by plan. If your EAC doesn't include a function described in this article, your plan might not include it.

For more information about the Exchange Online capabilities in your subscription plan, go to [What Microsoft 365 Apps for business product or license do I have?](https://support.office.com/article/f8ab5e25-bf3f-4a47-b264-174b1ee925fd) and [Exchange Online Service Description.](https://docs.microsoft.com/office365/servicedescriptions/exchange-online-service-description/exchange-online-service-description).

### Open the EAC, and confirm your admin role

To complete the tasks covered in this topic, [Use a screen reader to open the Exchange admin center](use-screen-reader-to-open-exchange-admin-center.md) and check that your global administrator has assigned you to the Organization Management and Records Management admin role groups. [Use a screen reader to identify your admin role in the Exchange admin center](use-screen-reader-to-identify-admin-role-in-exchange-admin-center.md).

## Create a mail flow rule to encrypt email messages

1. In the EAC, to move the focus to the first link in the navigation pane (**Dashboard**) press Ctrl+F6 *twice*. You hear "Dashboard, Primary navigation link..

2. To move the focus to the **mail flow** link in the navigation pane, press the Tab key until you hear "Mail flow, Primary navigation link." Press Enter.

3. To move the focus to the mail flow settings in the content area of the page, the first of which is the **rules** link, press Ctrl+F6. You hear "Rules, Secondary navigation link..

4. To create a new rule, move the focus to the **New** button by pressing the Tab key until you hear "New button." Press Enter. You hear "Menu." To select the **Create a new rule** option from the list of options that opens for the button, press the Down Arrow key. You hear "Create a new rule." Press Enter.

5. As the focus moves to the **Name** text box in the **new rule** pop-up window, you hear "New rule, Name, Edit." Type the name of the new rule (such as Encrypt email for email address). To move to the next option in the window, press the Tab key.

6. As the focus moves to the **Apply this rule if** drop-down box, you hear "Apply this rule if, Combo box." Press the Down Arrow or Up Arrow key until you hear the condition you want to select. Press Enter. For example, if you want to encrypt messages for a particular email address, perform the following five steps.

   1. In the **Apply this rule if** drop-down box, press the Down Arrow key until you hear "The recipient is." Press Enter.

   2. As the focus moves to the **Search** button in the **Select Members** pop-up window that opens, you hear "Select Members, Search..

   3. To move the focus to each of the following three elements of the user interface, press the Tab key:

      1. The **Display Name** column. You hear "Display Name, Column header..

      2. The list of names of each person in your organization in the **Name** column. You hear the name of the first person followed by "Button..

      3. The first person in the list. You hear the name of the first person followed by "Row."

   4. The first person in the list. You hear the name of the first person followed by "Row."

   5. To accept your changes, move the focus to the **OK** button by pressing the Tab key until you hear "Okay button." Press Enter.

7. As the focus moves back to the **new rule** pop-up window, you hear "New rule..

8. To move the focus to the **More options** link in the **new rule** pop-up window, press the Tab key until you hear "More options link." Press Enter.

   > [!TIP]
   > When you select the **More options** link, more user interface (UI) elements are added to the page and more options are added to the combo boxes. To have access to the **Modify the message security** option that you need to select in the next step, you must select the **More options** link.

9. To move the focus back to the **Do the following** drop-down box in the **new rule** pop-up window, press Shift+Tab until you hear "Do the following, Combo box." Perform the following two steps.

   1. In the **Do the following** drop-down box, to select the **Modify the message security** option, press the Down Arrow key until you hear "Modify the message security." Press Enter.

   2. As the focus moves to a list of message security options, you hear the first option in the list, "Apply rights protection." To select the **Apply Office 365 Message Encryption** option, press the Down Arrow key until you hear "Apply Office 365 Message Encryption." Press Enter.

10. To save the new rule, move the focus to the **Save** button by pressing the Tab key until you hear "Save button." Press Enter.

11. As the focus moves back to the **New** button on the **rules** content area of the page, you hear "Rules, New button." The new rule is turned on by default.

   > [!TIP]
   > To turn off a new rule, press the Tab key to tab through the elements of the **rules** content area of the page, use the Up Arrow and Down Arrow keys to select a rule, and then press Spacebar. To hear the settings for a selected rule, press the Tab key until the focus moves to the details pane for the selected rule, and you hear the details for the rule.

## Create a mail flow rule to decrypt email messages

1. In the EAC, to move the focus to the first link in the navigation pane (**Dashboard**) press Ctrl+F6 *twice*. You hear "Dashboard, Primary navigation link..

2. To move the focus to the **mail flow** link in the navigation pane, press the Tab key until you hear "Mail flow, Primary navigation link." Press Enter.

3. To move the focus to the mail flow settings in the content area of the page, the first of which is the **rules** link, press Ctrl+F6. You hear "Rules, Secondary navigation link..

4. To create a new rule, move the focus to the **New** button by pressing the Tab key until you hear "New button." Press Enter. You hear "Menu." To select the **Create a new rule** option from the list of options that opens for the button, press the Down Arrow key. You hear "Create a new rule." Press Enter.

5. As the focus moves to the **Name** text box in the **new rule** pop-up window, you hear "New rule, Name, Edit." Type the name of the new rule (such as Remove encryption from incoming mail). To move to the next option in the window, press the Tab key.

6. As the focus moves to the **Apply this rule if** drop-down box, you hear "Apply this rule if, Combo box." Press the Down Arrow or Up Arrow key until you hear the condition you want to select. Press Enter. For example, if you want to decrypt all incoming messages for your organization, perform the following four steps.

   1. In the **Apply this rule if** drop-down box, press the Down Arrow key until you hear "The recipient is located." Press Enter.

   2. As the focus moves to a list of locations in the **select recipient location** pop-up window that opens, you hear "Select recipient location..

   3. To move between and select a location in the list, press the Down Arrow and Up Arrow keys. You hear the name of each location. For example, to select the **Inside the organization location**, press the Down Arrow key until you hear "Inside the organization..

   4. To accept your changes, move the focus to the **OK** button by pressing the Tab key until you hear "Okay button." Press Enter.

7. As the focus moves back to the **new rule** pop-up window, you hear "New rule..

8. To move the focus to the **More options** link in the **new rule** pop-up window, press the Tab key until you hear "More options link." Press Enter.

   > [!TIP]
   > When you select the **More options** link, more user interface (UI) elements are added to the page and more options are added to the combo boxes. To have access to the **Modify the message security** option that you need to select in the next step, you must select the **More options** link.

9. To move the focus back to the **Do the following** drop-down box in the **new rule** pop-up window, press Shift+Tab until you hear "Do the following, Combo box." Perform the following two steps.

   1. In the **Do the following** drop-down box, to select the **Modify the message security** option, press the Down Arrow key until you hear "Modify the message security." Press Enter.

   2. As the focus moves to a list of message security options, you hear the first option in the list, "Apply rights protection." To select the **Remove Office 365 Message Encryption** option, press the Down Arrow key until you hear "Remove Office 365 Message Encryption." Press Enter.

10. To save the new rule, move the focus to the **Save** button by pressing the Tab key until you hear "Save button." Press Enter.

11. As the focus moves back to the **New** button on the **rules** content area of the page, you hear "Rules, New button." The new rule is turned on by default.

> [!TIP]
> To turn off a new rule, press the Tab key to tab through the elements of the **rules** content area of the page, use the Up Arrow and Down Arrow keys to select a rule, and then press Spacebar. To hear the settings for a selected rule, press the Tab key until the focus moves to the details pane for the selected rule, and you hear the details for the rule.
