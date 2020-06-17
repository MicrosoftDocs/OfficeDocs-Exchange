---
title: "Simplify the Outlook Web App URL for Office 365 Hybrid"
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
f1.keywords:
- NOCSH
audience: ITPro
ms.topic: article
ms.prod: exchange-server-it-pro
localization_priority: Normal
ms.collection:
- Hybrid
- M365-email-calendar
ms.assetid: 19449aee-3796-4298-90c6-c7579b8d2f7a
ms.reviewer: 
description: "Learn how to configure a URL for Outlook on the web (Outlook Web App) for cloud mailbox users in a hybrid environment."
---

# Simplify the Outlook Web App URL for Office 365 Hybrid

Learn how to configure a URL for Outlook on the web (Outlook Web App) for cloud mailbox users in a hybrid environment.

A major concern for organizations that move to Office 365 from on-premises Exchange is the user experience. Users need uninterrupted access to their mailboxes regardless of where or when their mailbox is moved. With that in mind, the Outlook on the web (formerly known as Outlook Web App) coexistence story is important.

Consider the following scenario: a company uses a hybrid deployment to move some of their mailboxes from on-premises Exchange to Office 365. On Friday before the move, the users accessed their on-premises mailboxes by using the URL https://mail.contoso.com/owa. On Monday after the move, those same users now get an error when they try to access their mailboxes by using that URL.

To allow the affected users to connect to their mailboxes by using Outlook on the web, you have two options:

- **Tell the users the new URL (for example, https://outlook.com/owa/contoso.com)**: The issues with this option are:

  - The URL is complex.

  - The experience isn't seamless for the affected users.

- **Configure the *TargetOWAUrl* setting on the organization relationship**: The issues with this option are:

  - The endpoint for the cloud mailboxes is external (it's not in the domain that users expect).

  - The endpoint requires the domain in the URL (to distinguish between Microsoft 365 Apps for business and outlook.com consumer offerings).

  - The endpoint causes users to see certificate mismatch warnings.

To eliminate these issues for users with cloud mailboxes, perform the following steps:

1. **Create a CNAME record in DNS (for example, cloudowa.contoso.com) that points to mail.office365.com**:

   - Be sure to create this CNAME record in your internal and external (public) DNS, because users might connect from internal or external Internet connections.

   - In our example, the domain contoso.com is used in the request (the cloudowa part is dropped). This means you don't need to specify the domain in the URL.

2. **Configure the Outlook on the web redirect in the on-premises organization relationship**: To do this, use the following syntax in the Exchange Management Shell in on-premises Exchange:

   ```
   Set-OrganizationRelationship -Identity <Organization relationship identity> -TargetOWAUrl http://<CNAME value>/owa
   ```

   For example, if the CNAME record that you created in Step 1 is cloudowa.contoso.com, and the organization relationship name is Contoso Cloud, run the following command:

   ```
   Set-OrganizationRelationship -Identity "Contoso Cloud" -TargetOWAUrl http://cloudowa.contoso.com/owa
   ```

   > [!NOTE]
   > • Use http, not https. <br/>• The trailing value /owa is required in the organization relationship, but users don't need to enter /owa in the URL. <br/>• You can use the [Get-OrganizationRelationship](https://docs.microsoft.com/powershell/module/exchange/get-organizationrelationship) cmdlet to find the organization relationship name.

## Multiple authentication prompts

Users may receive multiple authentication prompts depending on:

- Where they connect from (internal or external Internet connections).

- If their computer is or isn't domain joined.

- If you're using identity federation in your hybrid environment.

The authentication prompt experience that users can expect is described in the following table.

|**Authentication method**|**Client computer**|**Authentication prompt experience**|
|:-----|:-----|:-----|
|Identity federation|Internal Internet connection|Single prompt|
|Identity Federation|External Internet connection|Double prompt|
|No Identity federation|Domain joined (internal or external)|Double prompt|
|With or without identity federation|Not domain joined (internal or external)|Double prompt|

> [!NOTE]
> Identity federation requires that the AD FS endpoint is configured in the Intranet Zone of Internet Explorer as described in [KB2535227}(https://support.microsoft.com/help/2535227), and that AD FS is configured per the general Office 365 guidance.
