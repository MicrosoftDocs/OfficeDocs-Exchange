---
title: "Hybrid management in Exchange 2013/Exchange 2010 hybrid deployments"
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
- Ent_O365_Hybrid
- M365-email-calendar
ms.assetid: 613ad2c2-bb7a-4810-b572-71945bd103f1
ms.reviewer:
description: "When you install a server running Microsoft Exchange Server 2013 in your Exchange 2010 on-premises organization, the Exchange 2013 management tools are automatically installed on the server. You'll use the following tools to configure and manage hybrid functionality for both the on-premises Exchange and the Exchange Online organization:"
---

# Hybrid management in Exchange 2013/Exchange 2010 hybrid deployments

When you install a server running Microsoft Exchange Server 2013 in your Exchange 2010 on-premises organization, the Exchange 2013 management tools are automatically installed on the server. You'll use the following tools to configure and manage hybrid functionality for both the on-premises Exchange and the Exchange Online organization:

- **Exchange admin center**: The EAC is a web-based management console included with Exchange 2013 that's easy to use and is optimized for on-premises, online, or hybrid Exchange deployments. The EAC supplements the Exchange Management Console (EMC) and the Exchange Control Panel (ECP) interfaces that you use to manage Exchange Server 2010.

- **Exchange Management Shell**: The Exchange Management Shell is a Windows PowerShell-based command-line interface.

## The Exchange admin center

The EAC enables you to perform many deployment tasks and most common day-to-day administrative tasks on both the on-premises Exchange servers and the Exchange Online organization. It's installed by default on every Exchange 2013 server. In addition, because it's a web-based management console, you can also access it by using a web browser on other computers in your network or via the Internet by using the ECP virtual directory URL.

> [!IMPORTANT]
> If you want to access the EAC using an account with a mailbox located on an Exchange 2010 Mailbox server (such as a domain administrator account), you must use the following address in your browser to access the EAC: > https://_\<FQDN of Exchange 2013 Client Access server\>_/ECP?ExchClientVer=15

You access the Exchange Online organization in the EAC by selecting the Office 365 cross-premises navigation tab. The cross-premises navigation allows you to easily switch between your Exchange Online and your on-premises Exchange organizations. If you've configured a hybrid deployment, selecting the Office 365 tab allows you to manage the Exchange Online organization and recipient objects. If you don't have an Exchange Online organization, selecting the Office 365 link will direct you to the Office 365 sign-up page.

For more information about the EAC, see [Exchange admin center in Exchange 2013](https://docs.microsoft.com/exchange/exchange-admin-center-in-exchange-2013-exchange-2013-help).

## The Exchange Management Shell

The Exchange Management Shell enables you to perform any task that the EAC does and some additional tasks that can only be performed in the Exchange Management Shell. The Exchange Management Shell is a collection of Windows PowerShell scripts and cmdlets that are installed on a computer when the Exchange 2013 management tools are installed. These scripts and cmdlets are only loaded when you open the Exchange Management Shell using the Exchange Management Shell icon. If you open Windows PowerShell directly, the Exchange scripts and cmdlets aren't loaded and you won't be able to manage your on-premises organization.

> [!NOTE]
> You can create a manual Windows PowerShell connection to your local on-premises organization, similar to how you manually connect to the Exchange Online organization below. However, we strongly recommend that you use the Exchange Management Shell icon to open the Exchange Management Shell to manage your on-premises Exchange servers.

When you open the Exchange Management Shell using the Exchange Management Shell icon on a computer that has the management tools installed, you can manage your on-premises organization. However, you can't manage the Exchange Online organization when you open the Exchange Management Shell using this icon. This is because opening the Exchange Management Shell using the Exchange Management Shell icon automatically connects you to a local Exchange server.

If you want to manage the Exchange Online organization using Windows PowerShell, you must open Windows PowerShell directly and not via the Exchange Management Shell icon. When you open Windows PowerShell, you can then manually specify where you want to connect. When you create a manual connection, you specify an administrator account in the Office 365 organization, and then you run a command to create a connection. When the connection is established, the Exchange cmdlets you have permissions to run are made available to you. Learn more at [Connect to Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell).

If you're new to the Exchange Management Shell and want to learn the basics about how the Exchange Management Shell works, command syntax, and more, see [Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/exchange-management-shell).
