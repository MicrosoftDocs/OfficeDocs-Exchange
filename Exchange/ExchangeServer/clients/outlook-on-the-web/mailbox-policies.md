---
localization_priority: Normal
description: 'Summary: Learn how to configure Outlook on the web mailbox policies in Exchange Server 2016 or Exchange Server 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: be012ffe-8fdb-4fb7-aebd-78b3a55593fa
ms.reviewer:
title: View or configure Outlook on the web mailbox policy properties
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# View or configure Outlook on the web mailbox policy properties

You can configure mailbox policies in Exchange Server for Outlook on the web through the Exchange admin center (EAC) or Exchange Management Shell. After you create an Outlook on the web mailbox policy, you can then configure a variety of options to control the features available to users in Outlook on the web. For example, you can enable or disable Inbox rules or create a list of allowed file types for attachments.

## What do you need to know before you begin?

- Estimated time to complete each procedure: 3 minutes.

- To open the EAC, see [Exchange admin center in Exchange Server](../../architecture/client-access/exchange-admin-center.md). To open the Exchange Management Shell, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Outlook on the web mailbox policies" entry in the [Clients and mobile devices permissions](../../permissions/feature-permissions/client-and-mobile-device-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to view or configure Outlook on the web mailbox policies

1. In the EAC, click **Permissions** \> **Outlook Web App policies**.

2. In the result pane, click to select the mailbox policy you want to view or configure.

3. Click **Edit**.

4. On the **General** tab, you can view and edit the name of the policy.

5. On the **Features** tab, use the check boxes to enable or disable features. By default, the most common features are displayed. To see all features that can be enabled or disabled, click **More options**.

    **Notes**:

    - Features settings for Outlook on the web mailbox policies override Outlook on the web virtual directory settings. You can change segmentation settings for individual users by using the **Set-CASMailbox** cmdlet in the Exchange Management Shell.

    - The option to enable or disable the standard version of Outlook on the web by using the **Premium client** check box has been deprecated and will be removed from the settings. The standard version of Outlook on the web is always enabled.

6. On the **File Access** tab, use the check boxes to configure the file access and viewing options for users. File access lets a user open or view the contents of files attached to an email message.

    File access can be controlled based on whether a user has signed in on a public or private computer. The option for users to select private computer access or public computer access is available only when you're using forms-based authentication. All other forms of authentication default to private computer access.

    - **Direct file access**: Select this check box if you want to enable direct file access. Direct file access lets users open files attached to email messages.

    - **WebReady Document Viewing**: Select this check box if you want to enable supported documents to be converted to HTML and displayed in a web browser.

    - **Force WebReady Document Viewing when a converter is available**: Select this check box if you want to force documents to be converted to HTML and displayed in a web browser before users can open them in the viewing application. Documents can be opened in the viewing application only if direct file access has been enabled.

7. On the **Offline access** tab, use the option buttons to configure offline access availability.

8. Click **Save** to update the policy.

## Use the Exchange Management Shell to view Outlook on the web mailbox policies

This example retrieves the properties of the Outlook on the web mailbox policy `Executives` in the organization `Fabrikam`.

```powershell
Get-OwaMailboxPolicy -Identity Fabrikam\Executives
```

For more information about syntax and parameters, see [Get-OwaMailboxPolicy](https://docs.microsoft.com/powershell/module/exchange/get-owamailboxpolicy).

## Use the Exchange Management Shell to configure Outlook on the web mailbox policies

This example enables calendar access in the default mailbox policy.

```powershell
Set-OwaMailboxPolicy -Identity Default -CalendarEnabled $true
```

For more information about syntax and parameters, see [Set-OwaMailboxPolicy](https://docs.microsoft.com/powershell/module/exchange/set-owamailboxpolicy).

### How do you know this worked?

To verify that you've successfully edited an Outlook on the web mailbox policy:

1. In the EAC, click **Permissions** \> **Outlook Web App Policies**, and then choose a specific Outlook on the web mailbox policy.

2. Click **Edit** to view the properties of the mailbox policy.

3. Click **Save** or **Cancel** to close the properties page.

## See also

[Outlook Web App mailbox policy procedures in Exchange 2013](https://docs.microsoft.com/exchange/outlook-web-app-mailbox-policy-procedures-exchange-2013-help)
