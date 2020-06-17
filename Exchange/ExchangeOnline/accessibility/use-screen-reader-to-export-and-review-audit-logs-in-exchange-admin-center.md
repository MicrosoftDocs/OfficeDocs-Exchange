---
localization_priority: Normal
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 14828015-1e64-42eb-9814-2a8bc5b80bef
ms.reviewer: 
manager: serdars
description: Admins can learn how to use a screen reader to review and export the mailbox audit logs in the Exchange admin center (EAC) in Exchange Online.
title: Use a screen reader to export and review audit logs in the Exchange admin center
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
f1.keywords:
- CSH
ms.custom: A11y_UseSR
ms.service: exchange-online

---

# Use a screen reader to export and review audit logs in the Exchange admin center

You can export and review mailbox audit logs by using your screen reader in the Exchange admin center (EAC) in Exchange Online. When enabled, Exchange mailbox auditing logs information in the mailbox audit log whenever a user other than the owner accesses the mailbox. Each log entry includes information about who accessed the mailbox and the actions performed.

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

To export and review mailbox audit logs, [Use a screen reader to open the Exchange admin center](use-screen-reader-to-open-exchange-admin-center.md) and check that your global administrator has assigned you to the Organization Management and Records Management admin role groups. Learn how to [Use a screen reader to identify your admin role in the Exchange admin center](use-screen-reader-to-identify-admin-role-in-exchange-admin-center.md).

### Configure mailbox audit logging

Before you can export and review audit logs, you or another admin must enable mailbox audit logging and configure Outlook to allow XML attachments. These tasks are done in Exchange Online PowerShell. For more information, go to [Export mailbox audit logs in Exchange Online](../security-and-compliance/exchange-auditing-reports/export-mailbox-audit-logs.md).

## Export a mailbox audit log

1. In the EAC, press Ctrl+F6 until the primary navigation pane has the focus and you hear "Dashboard, Primary navigation link."

2. Tab to **compliance management** and press Enter.

3. To move to the menu bar, press Ctrl+F6.

4. Tab to **auditing**. You hear "Auditing, Secondary navigation link." Press Enter.

5. To access the main window list view, press Ctrl+F6. You hear "Audit reports..

6. Press the Tab key about six times until you hear " **Export mailbox audit logs**," and press Enter.

7. In the **Export Mailbox Audit Logs** dialog box which opens, the **Start date** year combo box has the focus, and you hear "Year of **Start date** combo box..

   > [!TIP]
   > By default, the start date is set to two weeks before yesterday's date. When enabled, the mailbox audit log typically stores entries for 90 days.

   1. If necessary, type the start date year for the audit logs. You can also select the start date year by pressing the Up Arrow key or the Down Arrow key.

   2. Tab to the **month** text box, and type or select the start date month.

   3. Tab to the **day** text box, and type or select the start date day.

8. Tab to the **End date** year combo box. You hear "Year of End date combo box..

   > [!TIP]
   > The default end date is today's date.

   1. If necessary, type the end date year for the audit logs. You can also select the end date year by pressing the Up Arrow key or the Down Arrow key.

   2. Tab to the **month** text box, and type or select the end date month.

   3. Tab to the **day** text box, and type or select the end date day.

9. To access the **select users** button, press the Tab key twice. You hear "Search these mailboxes or leave blank to find all mailboxes accessed by non-owners..

   > [!TIP]
   > If you want to export audit logs for all mailboxes, don't select any users, and go on to step 10. When the **Search these users** box is blank, the search includes all mailboxes.

   1. To open the **Select Mailbox** dialog box, with the focus on the **select users** button, press Enter. The **Search** box has the focus, and you hear "Filter or search edit." Type all or part of the name of the first mailbox whose audit logs you want to export and then, to search for the name, press Enter.

   2. To select a mailbox, press the Tab key four times until you hear the name of the mailbox owner in the search results list. If there are multiple mailboxes in the search results list, press the Down Arrow or Up Arrow key until you hear the name of the mailbox owner.

   > [!TIP]
   > You can select multiple consecutive mailboxes. To work with all mailboxes, leave the **Search** box blank, or enter all or part of the mailbox names you want to add. Tab to the search results. Press the Down Arrow key to hear each name. To add them all, press Ctrl+A. To add several mailboxes listed consecutively, press the Down Arrow key or the Up Arrow key until you hear the first mailbox name you want to add, hold down the Shift key, press the Down Arrow key or the Up Arrow key until you hear the last mailbox name you want to add, and then release the Shift key. All mailboxes between the first and last mailbox names are selected.

   3. To add the selected mailbox(es) to the list to be included in the audit log export, press Enter. The list of mailboxes retains the focus, so you can continue to add more mailboxes by selecting them and pressing Enter.

   > [!TIP]
   > To check the mailboxes you've added, tab to the **Add** button. To hear the list of mailboxes, press the Tab key again. You hear the first mailbox name in the list. To hear the second mailbox name in the list, press the Tab key one more time. Continue pressing the Tab key until you hear the names of all the mailboxes you've added. To delete a mailbox from the list, activate the **Remove** link by pressing Enter when you hear the mailbox name.

   4. To search for another mailbox or set of mailboxes, tab several times until you hear "Filter or search edit." Type all or part of the name of the next mailboxes you want to add, and press Enter. Repeat steps b and c. Do this for all mailboxes you want to add.

   5. To add an external mailbox, press the Tab key until you hear "Check names edit, Type in text." (In Narrator, you hear "Editing.") Type the email address of the external recipient, press Shift+Tab to select the **Check names** button, and then press Enter. This verifies the email address and adds it to the list of mailboxes.

   > [!TIP]
   > Be aware that if you type an external email address and press Enter, this adds the address to the list and then closes the dialog box. If you're not finished, use the **Check names** button to add it instead.

   6. When you finish adding mailboxes, tab to the **OK** button and press Enter. The **Export Mailbox Audit Logs** dialog box has the focus again, and the Search these mailboxes text box lists the selected mailboxes.

10. Tab to the **Search for access by** combo box. This specifies which types of mailbox non-owners you want the audit logs to show.

   - To have the audit logs show all non-owners, you don't need to do anything, as this is the default.

   - To specify a certain group of non-owners, like **External users** (Microsoft datacenter administrators), **Administrators and delegated users**, or **Administrators**, press the Down Arrow key to move to the user type you want, and then press Enter.

11. Press the Tab key twice to access the next **select users** button. You hear "Send the audit report to picker button." To open the **Select Members** dialog box, press Enter. The **Search** button has the focus.

   1. To search for a user within your organization, press Enter, type all or part of the name of the first audit log recipient, and then press Enter.

   2. Press the Tab key several times until you hear the name of the user in the search results list.

   3. To add the user to the list of audit log recipients, press the Down Arrow key until you hear the user's name, and then press Enter. The list of users retains the focus, so you can continue to add more recipients by selecting their mailboxes and pressing Enter.

   > [!TIP]
   > To check the recipients you've added, tab to the **Add** button. To hear the list of recipients, press the Tab key again. The first name is read. To hear the second name in the list, press the Tab key one more time. Continue pressing the Tab key until you hear the names of all the recipients you've added. To delete a recipient from the list, activate the **Remove** link by pressing Enter when you hear the username.

   4. To search for another name or set of names from within your organization, tab several times until you hear "Filter or search edit." Type all or part of the name of the next user you want to add, and press Enter. Repeat steps b and c. Do this for all audit report recipients in your organization.

   5. To add an external recipient, press the Tab key until you hear "Check names edit, Type in text." (In Narrator, you hear "Editing.") Type the email address of the external recipient, press Shift+Tab to select the **Check names** button, and then press Enter. This verified the email address and adds it to the list of recipients.

   > [!TIP]
   > Be aware that if you type an external email address and press Enter, this adds the recipient to the list and then closes the dialog box. If you're not finished, use the **Check names** button to add it instead.

   6. When you finish adding users, tab to the **OK** button and press Enter. The **Export** **Mailbox Audit Logs** dialog box has the focus again, and the **Send the audit report to** text box lists the audit log recipients.

12. Tab to the **export** button and press Enter. Exchange retrieves entries in the mailbox audit log that meet your search criteria, saves them to a file named SearchResult.xml, and then attaches the XML file to an email message sent within 24 hours to your selected audit log recipients.

   > [!TIP]
   > If you hear an error message that says the items you're trying to open couldn't be found, check that audit logging is enabled for the selected mailboxes. Also check that the selected dates are within range. The dates need to be after the date audit logging was enabled, and, by default, within the past 90 days.

## Review a mailbox audit log

1. Open Outlook and sign in to your mailbox (or the mailbox where the audit log was sent).

2. In the Inbox, find and open the message sent by Exchange or Outlook with a subject including "Mailbox Audit Log Search" and an XML file attachment named SearchResult.xml. The body of the email message contains the search criteria for this exported audit log.

   > [!TIP]
   > If Outlook is not configured to allow XML attachments, you might receive the email message but not be able to open the XML attachment. Also, if you can't find the message, you might need to wait longer. Recipients typically receive the exported audit log within 24 hours, but in some cases it might take a few days.

3. Select the message attachment and specify that you want to download the XML file.

4. Open the SearchResult.xml file in Excel. Each log entry includes information about non-owners of the mailbox who accessed the mailbox and the actions performed. The following fields are included, among others, in the audit log:

|**This mailbox audit log field**|**Gives this information**|
|:-----|:-----|
|Owner|The owner of the mailbox accessed by a non-owner|
|LastAccessed|The date and time of the most recent mailbox access|
|Operation|The action performed by the non-owner|
|OperationResult|Whether the action performed by the non-owner succeeded or failed|
|LogonType|The type of non-owner access, like administrator, delegate, or external Microsoft datacenter administrator|
|ClientIPAddress|The IP address of the computer used by the non-owner to access the mailbox|
|LogonUserDN|The display name of the non-owner|
|Subject|The subject line of the message affected by the non-owner|
