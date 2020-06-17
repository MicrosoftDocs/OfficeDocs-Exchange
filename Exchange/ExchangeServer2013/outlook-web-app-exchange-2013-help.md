---
title: 'Outlook Web App: Exchange 2013 Help'
TOCTitle: Outlook Web App
ms:assetid: 3814b665-01e8-4881-9a44-163f14789ee4
ms:mtpsurl: https://technet.microsoft.com/library/JJ657718(v=EXCHG.150)
ms:contentKeyID: 49300478
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Outlook Web App

_**Applies to:** Exchange Server 2013_

Learn about the Outlook Web App, which was called Outlook Web Access in versions of Microsoft Exchange earlier than 2010. This article provides a brief overview with links out to helpful information.

By default, when you install Microsoft Exchange 2013, you enable Outlook Web App. Microsoft Outlook Web App lets users access their Exchange mailbox from almost any Web browser.

The Client Access server role provides proxy and redirection services for Outlook Web App.

For information about new features, see [What's new in Exchange 2013](what-s-new-in-exchange-2013-exchange-2013-help.md). For information about the Client Access server role in Exchange 2013, see [Client Access server](client-access-server-exchange-2013-help.md).

## Overview of Outlook Web App

Fully supported web browsers give users access to features such as conversation view, Inbox rules, the reading pane, and the Scheduling Assistant. Browsers that aren't fully supported can still be used, but users will see the light version of Outlook Web App, which has fewer features. For information about new features in Outlook Web App, see [What's new for Outlook Web App in Exchange 2013](what-s-new-for-outlook-web-app-in-exchange-2013-exchange-2013-help.md).

## Managing Outlook Web App

In Exchange 2013, the most common Outlook Web App management tasks can be accomplished in the Exchange admin center (EAC). All these tasks, and many others, can be accomplished by using the Exchange Management Shell. You'll still use tools such as Internet Information Services (IIS) Manager for some tasks, for example, to configure Secure Sockets Layer (SSL) or set up simple URLs for users.

## Accessing Outlook Web App

You and your users can sign into Outlook Web App using a URL like this: `https://<domain name>/OWA` or `https://mail.<domain name>/OWA`.

If, for example, your organization's domain is contoso.com, then use <https://contoso.com/OWA> or <https://mail.contoso.com/OWA>. Learn more about accessing Outlook Web App [here](https://docs.microsoft.com/exchange/troubleshoot/client-connectivity/set-up-web-access). To change the sign-in URL to something different or to force redirection to SSL, see [Simplify the Outlook Web App URL](simplify-the-outlook-web-app-url-exchange-2013-help.md).

If you're using Exchange Online or Microsoft 365 or Office 365 for email, you and your users access Outlook on the web at **outlook.office365.com/owa** or in the [Microsoft 365 admin center](https://admin.microsoft.com). Learn more at [How to sign in to Outlook on the web](https://support.office.com/en-us/article/763fab4d-0138-4814-b450-37fc286bcb79) and [Where to sign in to Microsoft 365 for business](https://support.office.com/article/e9eb7d51-5430-4929-91ab-6157c5a050b4).
