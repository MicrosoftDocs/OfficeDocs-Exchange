---
title: "Use the Office 365 mail migration advisor"
ms.author: chrisda
author: chrisda
manager: dansimp
audience: ITPro
ms.topic: article
ms.prod: exchange-server-it-pro
localization_priority: Normal
ms.collection:
- Hybrid
- Ent_O365_Hybrid
- M365-email-calendar
description: "Use the Office 365 mail migration advisors to perform a hybrid, cutover, staged, Gmail, or IMAP, migration to Office 365."
---

# Use the Office 365 mail migration advisor

For migrations from an existing on-premises Exchange Server environment, you can migrate all email, calendar, and contacts from user mailboxes to Office 365. Office 365 provides a [mail migration advisor](https://aka.ms/MailSetupAdvisorFromEDA) to help you move mailboxes from your current mail system to Exchange Online in Office 365 with automated tools and step-by-step guidance. The advisor will recommend the best migration path for your organization based on your current mail system, the number of mailboxes you want to migrate, and how you plan to manage users and user access.

## How do I run the migration advisor?

To run the Office 365 migration advisor, you'll need the following:

- An Office 365 subscription plan that includes Exchange Online. To see which plans support Exchange Online, see [Office 365 Plan Options](https://docs.microsoft.com/office365/servicedescriptions/office-365-platform-service-description/office-365-plan-options) and [Exchange Online Service Description](https://docs.microsoft.com/office365/servicedescriptions/exchange-online-service-description/exchange-online-service-description).
- An Office 365 account with Global administrator permissions.

After you have everything you need, you can open the mail migration advisor by going to [Office 365 mail migration advisor](https://aka.ms/MailSetupAdvisorFromEDA). You'll need to log into your Office 365 tenant when you open this link.

## What options will I have when I run the migration advisor?

When you run the advisor, it'll ask you questions about your existing on-premises environment to help you figure out the best way to get your email to Office 365. Depending on your answers, the advisor will choose one of the following migration options:

- **Hybrid**:  Hybrid migrations come in a few different flavors - **full**, **minimal**, and **express**.

  - Full hybrid migrations are best for large organizations that have many thousands of mailboxes and need complete integration between their on-premises Exchange organization and Office 365. Active Directory is synchronized with Office 365, free/busy information can be exchanged, enhanced mail flow options become available, and more. If you want to learn more about full hybrid migrations, see [Exchange hybrid deployments](https://docs.microsoft.com/exchange/exchange-hybrid).

  - Minimal hybrid migrations are best for medium-sized organizations that have a few hundred to a couple thousand mailboxes and want to finish their migration within a couple months. Like full hybrid, minimal hybrid migrations set up on-going Active Directory synchronization with Office 365 to help with recipient administration. However, features like free/busy synchronization and other enhanced features aren't available. If you want to learn more about minimal hybrid migrations, see [Minimal Hybrid Configuration](https://blogs.technet.microsoft.com/exchange/2016/06/24/hcw-improvement-the-minimal-hybrid-configuration-option/).

  - Express migrations are best for small organizations that want to finish their migration within a couple weeks. Express migration does a one-time Active Directory synchronization with Office 365 to set up recipients and then helps move their mailboxes to Office 365. No enhanced features are available. If you want to learn more about express hybrid migrations, see [Express Hybrid Migrations](https://blogs.technet.microsoft.com/exchange/2016/11/28/new-exchange-online-migration-options/).

- **Staged**: Staged migrations are best for organizations running Exchange 2003 or Exchange 2007 with fewer than 2000 mailboxes who want to move mailboxes over in batches over a couple weeks. While you can do staged migrations with later versions of Exchange, we strongly recommend that you use the **Minimal** hybrid migration option instead. With staged migrations, you perform a one-time Active Directory synchronization with Office 365 to create recipients in Office 365. When that's complete, you can select batches of mailboxes to move to Office 365 at a time. After a mailbox is moved, users are provided with new login credentials and need to reconfigure their Outlook and other mail app profiles. For more information about staged migrations, check out[ What you need to know about a staged email migration to Office 365](https://docs.microsoft.com/exchange/mailbox-migration/what-to-know-about-a-staged-migration).

- **Cutover**: Cutover migrations are best for organizations running Exchange 2003 or Exchange 2007 with fewer than 2000 mailboxes who want to move mailboxes over a couple days. While you can do cutover migrations with later versions of Exchange, we strongly recommend that you use the **Express** hybrid migration option instead. With cutover migrations, Office 365 reads your Active Directory and creates new recipients in Office 365. When that's complete, Office 365 copies the contents of all users' mailboxes to their new mailboxes in Office 365. When complete, users are provided with new login credentials and need to reconfigure their Outlook and other mail app profiles. For more information about cutover migrations, check out [What you need to know about a cutover email migration to Office 365](https://docs.microsoft.com/exchange/mailbox-migration/what-to-know-about-a-cutover-migration).

If you want to learn more about these options or need more help deciding which option to use, check out [Ways to migrate multiple email accounts to Office 365](https://docs.microsoft.com/exchange/mailbox-migration/mailbox-migration) and [Decide on a migration path](https://docs.microsoft.com/exchange/mailbox-migration/decide-on-a-migration-path).
