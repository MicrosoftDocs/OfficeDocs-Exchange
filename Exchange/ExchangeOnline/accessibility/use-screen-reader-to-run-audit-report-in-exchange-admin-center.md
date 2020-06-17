---
localization_priority: Normal
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: cdf7bb59-e1a5-457c-9a59-558904fbd68c
ms.reviewer: 
manager: serdars
description: Admins can learn how to use a screen reader to run audit reports in the Exchange admin center (EAC) in Exchange Online.
title: Use a screen reader to run an audit report in the Exchange admin center in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
f1.keywords:
- CSH
ms.custom: A11y_UseSR
ms.service: exchange-online

---

# Use a screen reader to run an audit report in the Exchange admin center in Exchange Online

You can run audit reports and search for audit information by using your screen reader in the Exchange admin center (EAC) in Exchange Online. Certain audit reports can help you troubleshoot configuration issues by tracking specific changes made by administrators. Other audit reports can help you monitor regulatory, compliance, and litigation requirements.

## Get started

Navigate with Internet Explorer and keyboard shortcuts, and make sure that you have the appropriate Office 365 or Microsoft 365 subscription plan and admin role to work in the EAC. Then, open the EAC and get started.

### Use your browser and keyboard to navigate in the EAC

Exchange Online, which includes the EAC, is a web-based application, so the keyboard shortcuts and navigation may be different from those in Exchange 2016. [Accessibility in the Exchange admin center](accessibility-in-exchange-admin-center.md).

For best results when working in the EAC in Exchange Online, use Internet Explorer as your browser. [Learn more about Internet Explorer keyboard shortcuts](https://support.microsoft.com/help/17456/).

Many tasks in the EAC require the use of pop-up windows so, in your browser, be sure to [enable pop-up windows for Office 365](https://support.microsoft.com/help/17479).

### Confirm your Office 365 or Microsoft 365 subscription plan

For more information about the Exchange Online capabilities in your subscription plan, go to [What Office 365 business product or license do I have?](https://support.office.com/article/f8ab5e25-bf3f-4a47-b264-174b1ee925fd) and [Exchange Online Service Description.](https://docs.microsoft.com/office365/servicedescriptions/exchange-online-service-description/exchange-online-service-description).

### Open the EAC, and confirm your admin role

To run audit reports, [Use a screen reader to open the Exchange admin center](use-screen-reader-to-open-exchange-admin-center.md) and check that your global administrator has assigned you to the Organization Management and Records Management admin role groups. To run In-Place eDiscovery or In-Place Hold reports, check that you are assigned to the Discovery Management role group. Learn how to [Use a screen reader to identify your admin role in the Exchange admin center](use-screen-reader-to-identify-admin-role-in-exchange-admin-center.md).

## Find data to troubleshoot configuration and security issues

Troubleshoot configuration issues by examining logged information about mailbox access by non-owners, Exchange Online configuration changes, and administrator role group updates. This information is available on the **Compliance Management** tab and the **Auditing** page of the EAC.

### Search for non-owner mailbox access

When Exchange mailbox auditing is enabled for a mailbox, information is recorded in the mailbox audit log whenever a user other than the owner accesses that mailbox. Each log entry includes information about who accessed the mailbox and what actions were performed. Search for non-owner mailbox access when you need to troubleshoot possible security issues.

> [!NOTE]
> Before you can search for non-owner mailbox access, you or another Admin must enable mailbox audit logging, which is done in Exchange Online PowerShell. [Learn more about running a non-owner mailbox access report](https://go.microsoft.com/fwlink/p/?LinkId=799418).

1. In the EAC, press Ctrl+F6 until the primary navigation pane has the focus and you hear "Dashboard, Primary navigation link."

2. Tab to **compliance management** and press Enter.

3. To move to the menu bar, press Ctrl+F6.

4. Tab to **auditing**. You hear "Auditing, Secondary navigation link." Press Enter.

5. To access the main window list view, press Ctrl+F6. You hear "Audit reports..

6. Press the Tab key about three times until you hear "Run a non-owner mailbox access report." Press Enter.

7. In the **Search for Mailboxes Accessed by Non-Owners** dialog box which opens, the **Start date year** combo box has the focus, and you hear "Year of Start date combo box..

   > [!TIP]
   > By default, the start date is set to two weeks before yesterday's date. When enabled, the mailbox audit log typically stores entries for 90 days.

   1. If necessary, type the start date year for your administrator configuration change search. You can also select the start date year by pressing the Up Arrow key or the Down Arrow key.

   2. Tab to the **month** text box, and type or select the start date month.

   3. Tab to the **day** text box, and type or select the start date day.

8. Tab to the **End date year** combo box. You hear "Year of End date combo box..

   > [!TIP]
   > The default end date is today's date.

   1. If necessary, type the end date year for your administrator configuration change search. You can also select the end date year by pressing the Up Arrow key or the Down Arrow key.

   2. Tab to the **month** text box, and type or select the end date month.

   3. Tab to the **day** text box, and type or select the end date day.

9. Press the Tab key to access the **search** button, and press Enter.

   > [!TIP]
   > If you want to search all mailboxes for non-owner access, don't select any specific mailboxes, and go on to step 10. When the **Search these mailboxes** box is blank, the search includes all mailboxes.

   1. To open the **Select Mailbox** dialog box, with the focus on the select mailboxes button, press Enter. The **Search** box has the focus, and you hear "Filter or search edit." Type all or part of the name of the first mailbox you want to include in the non-owner mailbox access search and then, to search for the name, press Enter.

   2. To select a mailbox, press the Tab key about four times until you hear the name of the mailbox owner in the search results list. If there are multiple mailboxes in the search results list, press the Down Arrow key or Up Arrow key until you hear the name of the mailbox owner.

   > [!TIP]
   > You can select multiple consecutive mailboxes. To work with all mailboxes, leave the **Search** box blank, or enter all or part of the mailbox names you want to add. Tab to the search results. Press the Down Arrow key to hear each name. To add them all, press Ctrl+A. To add several mailboxes listed consecutively, press the Down Arrow key or the Up Arrow key until you hear the first mailbox name you want to add, hold down the Shift key, press the Down Arrow key or the Up Arrow key until you hear the last mailbox name you want to add, and then release the Shift key. All mailboxes between the first and last mailbox names are selected.

   3. To add the selected mailbox(es) to the list to be included in the non-owner mailbox access search, press Enter. The list of mailboxes retains the focus, so you can continue to add more mailboxes by selecting them and pressing Enter.

   > [!TIP]
   > To check the mailboxes you've added, tab to the **Add** button. To hear the list of mailboxes, press the Tab key again. You hear the first mailbox name in the list. To hear the second mailbox name in the list, press the Tab key once more. Continue pressing the Tab key until you hear the names of all the mailboxes you've added. To delete a mailbox from the list, activate the **Remove** link by pressing Enter when you hear the mailbox name.

   4. To search for another mailbox or set of mailboxes, tab several times until you hear "Filter or search edit." Type all or part of the name of the next mailboxes you want to add, and press Enter. Repeat steps b and c. Do this for all mailboxes you want to add.

   5. To add an external mailbox, press the Tab key until you hear "Check names edit, Type in text." (In Narrator, you hear "Editing.") Type the email address of the external recipient, press Shift+Tab to select the **Check names** button, and then press Enter. This verifies the email address and adds it to the list of mailboxes.

   > [!TIP]
   > Be aware that if you type an external email address and press Enter, this adds the address to the list and then closes the dialog box. If you're not finished, use the **Check names** button to add it instead.

   6. When you finish adding mailboxes, tab to the **OK** button and press Enter. The **Search for Mailboxes Accessed by Non-Owners** dialog box has the focus again, and the **Search these mailboxes** text box lists the selected mailboxes.

10. Tab to the **Search for access by** combo box. This specifies which types of mailbox non-owners you want the non-owner mailbox report to show.

   - To search the audit logs for administrator access, you don't need to do anything, as this is the default.

   - To search the audit logs for another group of non-owners, like **All non-owners**, **External users** (Microsoft datacenter administrators), or **Administrators and delegated users**, press the Up Arrow key to move to the user type you want.

11. Press the Tab key to access the **Search** button, and press Enter.

12. Press the Tab key about four times to access the search results. If any mailboxes were accessed by a non-owner of the type you specified in the time period you selected, you hear the name of the mailbox owner and the date the mailbox was accessed by a non-owner. If none of the mailboxes were accessed by a non-owner, you hear "There are no items to show in this view." (In Narrator, you hear "Contains 0 items.")

13. For more details about a non-owner mailbox access, with the item selected in the search results list, press the Tab key to move to the **details** pane. To print the contents of the **details** pane, press Enter. To hear the contents of the **details** pane, press Tab again.

14. To close the dialog box, tab to the **Close** button and press Enter.

   > [!TIP]
   > You can also export the log of non-owner access of mailboxes and review it in an XML file. Learn more in [Use a screen reader to export and review audit logs in the Exchange admin center](use-screen-reader-to-export-and-review-audit-logs-in-exchange-admin-center.md).

### Search for configuration changes on a mailbox

With administrator audit logging, Exchange records specific changes an administrator makes to the organization's Exchange configuration. Such changes can include adding users, adding public folders, creating policies or rules, and so on. This can help you troubleshoot configuration problems or identify the cause of security-related or compliance-related problems. [Learn more about viewing the administrator audit log](https://go.microsoft.com/fwlink/p/?LinkId=799437).

1. In the EAC, press Ctrl+F6 until the primary navigation pane has the focus and you hear "Dashboard, Primary navigation link."

2. Tab to **compliance management** and press Enter.

3. To move to the menu bar, press Ctrl+F6.

4. Tab to **auditing**. You hear "Auditing, Secondary navigation link." Press Enter.

5. To access the main window list view, press Ctrl+F6. You hear "Audit reports..

6. Press the Tab key about 12 times until you hear "Run the admin audit log report." Press Enter.

7. In the **View the Administrator Audit Log** dialog box which opens, the **Start date year** combo box has the focus, and you hear "Year of Start date combo box..

   > [!TIP]
   > By default, the start date is set to two weeks before yesterday's date. The administrator audit log typically stores entries for 90 days.

   1. If necessary, type the start date year for your administrator configuration change search. You can also select the start date year by pressing the Up Arrow key or the Down Arrow key.

   2. Tab to the **month** text box, and type or select the start date month.

   3. Tab to the **day** text box, and type or select the start date day.

8. Tab to the **End date year** combo box. You hear "Year of End date combo box..

   > [!TIP]
   > The default end date is today's date.

   1. If necessary, type the start date year for your administrator configuration change search. You can also select the end date year by pressing the Up Arrow key or the Down Arrow key.

   2. Tab to the **month** text box, and type or select the end date month.

   3. Tab to the **day** text box, and type or select the end date day.

9. Press the Tab key to access the **search** button, and press Enter.

10. Press the Tab key about five times to access the search results. Press the Down Arrow key or the Up Arrow key to hear the list of configuration changes made in the time period you specified. For each item, you hear the date of the change, the type of configuration change made, and the name of the Administrator who made the change. If there were no configuration changes, you hear "There are no items to show in this view." (In Narrator, you hear "Contains 0 items.")

11. For more details about a configuration change, with the change selected in the search results list, press the Tab key to move to the **details** pane. To print the contents of the **details** pane, press Enter. To hear the contents of the **details** pane, press Tab again.

12. To close the dialog box, tab to the **Close** button and press Enter.

   > [!TIP]
   > You can also export the admin audit log to an XML file and email it to specified recipients. On the auditing page, press the Tab key until you hear "Export the admin audit log." Press Enter and work through the **Export the Administrator Audit Log** dialog box which appears. For more information, go to [Use a screen reader to export and review audit logs in the Exchange admin center](use-screen-reader-to-export-and-review-audit-logs-in-exchange-admin-center.md).

### Search for administrator role group changes

You can search for administrator role changes, which, like configuration changes, are recorded in the administrator audit log. With a targeted search, you can examine the admin audit log for changes made to role groups, which are used to assign administrative permissions to users. [Learn more about running an administrator role group report](https://go.microsoft.com/fwlink/p/?LinkId=799420).

1. In the EAC, press Ctrl+F6 until the primary navigation pane has the focus and you hear "Dashboard, Primary navigation link."

2. Tab to **compliance management** and press Enter.

3. To move to the menu bar, press Ctrl+F6.

4. Tab to auditing. You hear "Auditing, Secondary navigation link." Press Enter.

5. To access the main window list view, press Ctrl+F6. You hear "Audit reports..

6. Press the Tab key about nine times until you hear "Run an administrator role group report." Press Enter.

7. In the **Search for Changes to Administrative Role Groups** dialog box which opens, the **Start date year** combo box has the focus, and you hear "Year of Start date combo box..

   > [!TIP]
   > By default, the start date is set to two weeks before yesterday's date. The administrator audit log typically stores entries for 90 days.

   1. If necessary, type the start date year for your administrator role group change search. You can also select the start date year by pressing the Up Arrow key or the Down Arrow key.

   2. Tab to the **month** text box, and type or select the start date month.

   3. Tab to the **day** text box, and type or select the start date day.

8. Tab to the **End date year** combo box. You hear "Year of End date combo box..

   > [!TIP]
   > The default end date is today's date.

   1. If necessary, type the start date year for your administrator role group change search. You can also select the end date year by pressing the Up Arrow key or the Down Arrow key.

   2. Tab to the **month** text box, and type or select the end date month.

   3. Tab to the **day** text box, and type or select the end date day.

9. To access the **select role groups** button, press the Tab key twice. You hear "Search these role groups or leave this box blank to find all changed role groups..

   > [!TIP]
   > If you want to search all role groups for changes, don't select any specific role groups, and go on to step 10. When the **Search these role groups** box is blank, the search includes all role groups.

   1. To open the **Select a Role** dialog box, with the focus on the select role groups button, press Enter. The **Search** box has the focus, and you hear "Filter or search edit." Type all or part of the name of the first role group you want to include in the search and then, to search for the role group, press Enter.

   2. To select a role group, press the Tab key about three times until you hear the name of the role group in the search results list. If there are role groups in the search results list, press the Down Arrow key or Up Arrow key until you hear the name of the role group.

   > [!TIP]
   > You can select multiple consecutive role groups. To work with all role groups, leave the **Search** box blank, or enter all or part of the role group names you want to add. Tab to the search results. Press the Down Arrow key to hear each name. To add them all, press Ctrl+A. To add several role groups listed consecutively, press the Down Arrow key or the Up Arrow key until you hear the first role group name you want to add, hold down the Shift key, press the Down Arrow key or the Up Arrow key until you hear the last role group name you want to add, and then release the Shift key. All role groups between the first and last names are selected.

   3. To add the selected role group(s) to the list to be included in the role group change search, press Enter. The list of role groups retains the focus, so you can continue to add more role groups by selecting them and pressing Enter.

   > [!TIP]
   > To check the role groups you've added, tab to the **Add** button. To hear the list of role groups, press the Tab key again. You hear the first role group name in the list. To hear the second role group name in the list, press the Tab key once more. Continue pressing the Tab key until you hear the names of all the role groups you've added. To delete a role group from the list, activate the **Remove** link by pressing Enter when you hear the role group name.

   4. When you finish adding role groups, tab to the **OK** button and press Enter. The **Search for Changes to Administrator Role Groups** dialog box has the focus again, and the **Search these role groups** text box lists your selected role groups.

10. Press the Tab key to access the **Search** button, and press Enter.

11. Press the Tab key about four times to access the search results. If any of your selected role groups were changed in the time period you selected, you hear the name of the role group and the date of the change. If none of the role groups were changed, you hear "There are no items to show in this view." (In Narrator, you hear "Contains 0 items.")

12. For more details about a role group change, with the change selected in the search results list, press the Tab key to move to the **details** pane. To print the contents of the details pane, press Enter. To hear the contents of the **details** pane, press Tab again.

13. To close the dialog box, tab to the **Close** button and press Enter.

## Find data about changes to compliance status

Monitor regulatory, compliance, and litigation requirements by finding status changes to In-Place eDiscovery and Hold and the Per-mailbox Litigation Hold. This information is available on the **Compliance Management** tab and the **Auditing** page of the EAC.

### Search for changes to In-Place eDiscovery and Hold status

If your organization adheres to legal discovery requirements (related to organizational policy, compliance, or lawsuits), In-Place eDiscovery and In-Place Hold in Exchange Online can help you perform discovery searches for relevant content within mailboxes. You can search the administrator audit log to find mailboxes that have been put on or removed from In-Place eDiscovery or In-Place Hold. [Learn more about In-Place eDiscovery & Hold reports](https://go.microsoft.com/fwlink/p/?LinkId=799419).

1. In the EAC, press Ctrl+F6 until the primary navigation pane has the focus and you hear "Dashboard, Primary navigation link."

2. Tab to **compliance management** and press Enter.

3. To move to the menu bar, press Ctrl+F6.

4. Tab to **auditing**. You hear "Auditing, Secondary navigation link." Press Enter.

5. To access the main window list view, press Ctrl+F6. You hear "Audit reports..

6. Press the Tab key about 15 times until you hear "Run an In-Place eDiscovery and Hold report." Press Enter.

7. In the **Search for changes to In-Place eDiscovery & Hold** dialog box which opens, the Start date year combo box has the focus, and you hear "Year of Start date combo box..

   > [!TIP]
   > By default, the start date is set to two weeks before yesterday's date. The administrator audit log typically stores entries for 90 days.

   1. If necessary, type the start date year for the eDiscovery and Hold change search. You can also select the start date year by pressing the Up Arrow key or the Down Arrow key.

   2. Tab to the **month** text box, and type or select the start date month.

   3. Tab to the **day** text box, and type or select the start date day.

8. Tab to the **End date year** combo box. You hear "Year of End date combo box..

   > [!TIP]
   > The default end date is today's date.

   1. If necessary, type the end date year for your eDiscovery and Hold change search. You can also select the end date year by pressing the Up Arrow key or the Down Arrow key.

   2. Tab to the **month** text box, and type or select the end date month.

   3. Tab to the **day** text box, and type or select the end date day.

9. Press the Tab key to access the **Search** button, and press Enter.

10. Press the Tab key about three times to access the search results. If any eDiscovery or Holds were changed in the time period you selected, you hear their names. If none have been changed, you hear "There are no items to show in this view." (In Narrator, you hear "Contains 0 items.")

11. For more details about an eDiscovery or Hold change, with the change selected in the search results list, press the Tab key to move to the details pane. To print the contents of the **details** pane, press Enter. To hear the contents of the **details** pane, press Tab again.

12. To close the dialog box, tab to the **Close** button and press Enter.

### Search for mailboxes that are enabled or disabled for litigation holds

If your organization is involved in a legal action, you may have to take steps to preserve email messages that might be used as evidence. You can use the litigation hold feature to retain all email sent and received by specific people or retain all email sent and received in your organization for a specific time period. Search the administrator audit log to monitor the mailboxes that have had a change to their litigation hold status (enabled or disabled) during a specified time period. Learn more about [running a per-mailbox litigation hold report](https://go.microsoft.com/fwlink/p/?LinkId=799414).

1. In the EAC, press Ctrl+F6 until the primary navigation pane has the focus and you hear "Dashboard, Primary navigation link."

2. Tab to **compliance management** and press Enter.

3. To move to the menu bar, press Ctrl+F6.

4. Tab to **auditing**. You hear "Auditing, Secondary navigation link." Press Enter.

5. To access the main window list view, press Ctrl+F6. You hear "Audit reports..

6. Press the Tab key about 21 times until you hear "Run a per-mailbox Litigation Hold report." Press Enter.

7. In the **Search for Changes to Per-Mailbox Litigation Hold** dialog box which opens, the **Start date year** combo box has the focus, and you hear "Year of Start date combo box..

   > [!TIP]
   > By default, the start date is set to two weeks before yesterday's date. The administrator audit log typically stores entries for 90 days.

   1. If necessary, type the start date year for your litigation hold change search. You can also select the start date year by pressing the Up Arrow key or the Down Arrow key.

   2. Tab to the **month** text box, and type or select the start date month.

   3. Tab to the **day** text box, and type or select the start date day.

8. Tab to the **End date year** combo box. You hear "Year of End date combo box..

   > [!TIP]
   > The default end date is today's date.

   1. If necessary, type the end date year for your litigation hold change search. You can also select the end date year by pressing the Up Arrow key or the Down Arrow key.

   2. Tab to the **month** text box, and type or select the end date month.

   3. Tab to the **day** text box, and type or select the end date day.

9. To access the select users button, press the Tab key twice. You hear "Search these mailboxes or leave blank to find all mailboxes with litigation hold changes..

   > [!TIP]
   > If you want to search all mailboxes for litigation hold changes, don't select any specific mailboxes, and go on to step 10. When the **Search these mailboxes** box is blank, the search includes all mailboxes.

   1. To open the **Select Members** dialog box, with the focus on the select users button, press Enter. The **Search** button has the focus. To search for a user within your organization, press the Spacebar, type all or part of the name of the user, and then press Enter.

   2. Press the Tab key about seven times until you hear the name of the user in the search results list.

   3. To add the user to the list of mailboxes in the litigation hold search, press the Down Arrow key until you hear the user's name, and then press Enter. The list of users retains the focus, so you can continue to add more users by selecting their mailboxes and pressing Enter.

   > [!TIP]
   > To check the users you've added, tab to the **Add** button. To hear the list of users, press the Tab key again. The first name is read. To hear the second name in the list, press the Tab key once more. Continue pressing the Tab key until you hear the names of all the users you've added. To delete a user from the list, activate the **Remove** link by pressing Enter when you hear the username.

   4. To add an external user, press the Tab key until you hear "Check names edit, Type in text." (In Narrator, you hear "Editing.") Type the email address of the external user, press Shift+Tab to select the **Check names** button, and then press Enter. This verifies the email address and adds it to the list of users.

   > [!TIP]
   > Be aware that if you type an external email address and press Enter, this adds the user to the list and then closes the dialog box. If you're not finished, use the **Check names** button to add it instead.

   5. When you finish adding users, tab to the **OK** button and press Enter. The **Search for Changes to Per-Mailbox Litigation Hold** dialog box has the focus again, and the **Search these mailboxes** text box lists the mailboxes to be searched for litigation hold changes.

10. Press the Tab key to access the **Search** button, and press Enter.

11. Press the Tab key about three times to access the search results. If any mailboxes had a change to its litigation hold status in the time period you selected, you hear the name of the mailbox owner. If none of the mailboxes were accessed by a non-owner, you hear "There are no items to show in this view." (In Narrator, you hear "Contains 0 items.")

12. For more details about a litigation hold change, with the change selected in the search results list, press the Tab key to move to the details pane. To print the contents of the details pane, press Enter. To hear the contents of the details pane, press Tab again.

13. To close the dialog box, tab to the **Close** button and press Enter.
