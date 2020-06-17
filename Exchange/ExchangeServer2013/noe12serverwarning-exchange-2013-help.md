---
title: "Can't install Exchange 2007 roles after preparing Active Directory for Exchange 2010"
TOCTitle: Cannot install Exchange 2007 roles after you prepare Active Directory for Exchange 2010_NoE12ServerWarning
ms:assetid: 4e579f69-0de9-421c-ba31-4e63a25e6a45
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.noe12serverwarning(v=EXCHG.150)
ms:contentKeyID: 46628900
ms.reviewer:
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Cannot install Exchange 2007 roles after you prepare Active Directory for Exchange 2010\_NoE12ServerWarning

_**Applies to:** Exchange Server 2013_

The content in this topic hasn't been updated for Microsoft Exchange Server 2013. While it hasn't been updated yet, it may still be applicable to Exchange 2013. If you still need help, check out the community resources below.

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

When you run Microsoft Exchange Server 2010 **Setup /PrepareAD**, the Microsoft Exchange Server Analyzer Tool queries the existing Active Directory topology to determine whether any Microsoft Exchange Server 2007 server roles exist. If Exchange 2007 server roles are not detected, you receive the following warning message:

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>Setup is going to prepare the organization for Exchange Server 2010 via 'Setup /PrepareAD' and no Exchange Server 2007 roles have been detected in this topology. After this operation, it will be impossible to install any Exchange Server 2007 roles.</p></td>
</tr>
</tbody>
</table>

Before deploying Exchange Server 2010, consider the following factors that may require you to deploy an Exchange 2007 server with all server roles installed prior to deploying Exchange 2007:

- **Third-party or in-house developed applications**: Applications developed for Exchange 2003 may not be compatible with Exchange 2010, and thus need to be upgraded or replaced. You can maintain these applications and the associated user population on Exchange 2003; move to Exchange 2007; or replace the software with a compatible version for Exchange 2010.

- **Coexistence or migration requirements**: If you plan on migrating mailboxes into your organization, you can either deploy Exchange 2007 and use the Microsoft Transporter Suite, or you can use a third-party coexistence or migration solution. To download the Microsoft Transporter Suite, go to [Microsoft Transporter Suite](https://go.microsoft.com/fwlink/?linkid=82688) at the Microsoft Download Center.

In addition, when evaluating the options for your organization, make sure you have considered the following questions:

- Do you have a strategy in place to move the dependent applications to Exchange 2010 before Exchange 2003 reaches end of support? For more information, visit the Microsoft Support Lifecycle Web page (<https://docs.microsoft.com/lifecycle/>).

- Does your strategy require WebDAV and Web Services coexistence (Exchange 2007)?

- Have you considered third-party products that support Exchange or other Microsoft technologies that will allow you to meet your coexistence or migration requirements?

- What is your hardware lifecycle approach (continue to use as many existing 32-bit servers as possible versus buying new 64-bit servers)?

- What is your plan for migration (migrate all servers as fast as possible versus migrating in a phased strategy)? Similarly, what is your timeline for the coexistence of different versions of Exchange?

If you decide that you need to deploy an Exchange 2007 server prior to deploying Exchange 2010, the deployment of a single Exchange 2007 with all server roles is sufficient to enable the deployment of future Exchange 2007 servers in the organization. To deploy the Exchange 2007 server into your Exchange 2003 organization, follow these steps:

1. Run Exchange 2007 **Setup /PrepareSchema**.

2. Run Exchange 2007 **Setup /PrepareAD**.

3. Run Exchange 2007 **Setup /PrepareDomain** on all domains that contain recipients, Exchange 2003 servers, or global catalogs that could be used by an Exchange server.

4. Install an Exchange 2007 server with all four server roles (Hub Transport, Client Access, Mailbox, and Unified Messaging).
