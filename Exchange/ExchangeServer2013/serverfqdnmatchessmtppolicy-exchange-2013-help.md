---
title: 'Fully-qualified domain name of the computer matches a recipient policy'
TOCTitle: The fully-qualified domain name of the computer matches a recipient policy_ServerFQDNMatchesSMTPPolicy
ms:assetid: f3ea61f8-1788-4cbf-814e-f7c088c1ac47
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.serverfqdnmatchessmtppolicy(v=EXCHG.150)
ms:contentKeyID: 46629176
ms.reviewer:
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# The fully-qualified domain name of the computer matches a recipient policy\_ServerFQDNMatchesSMTPPolicy

_**Applies to:** Exchange Server 2013_

The content in this topic hasn't been updated for Microsoft Exchange Server 2013. While it hasn't been updated yet, it may still be applicable to Exchange 2013. If you still need help, check out the community resources below.

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

Microsoft® Exchange Server 2007 setup cannot continue because the Fully Qualified Domain Name (FQDN) of the local computer matches the Simple Mail Transfer Protocol (SMTP) address of a recipient policy.

Microsoft Exchange setup requires that the FQDN of the servers in an Exchange organization not match any SMTP addresses of recipient policies in the same Exchange organization.

If the FQDN of a computer matches the SMTP address of a recipient policy, this match can cause mail to fail over SMTP and stall in the MTA queues.

To resolve this issue, rename the local computer or remove or rename the recipient policy and rerun Microsoft Exchange setup.

**To rename the local computer**

1. Open **System** in **Control Panel**.

2. On the **Computer Name** tab, click **Change**.

3. Under **Computer name**, type a new name for the computer, and then click **OK**. You will be prompted to provide a username and user password to rename the computer in the domain.

4. Click **OK** to close the **System Properties** dialog box. You will be prompted to restart your computer to apply your changes.

> [!IMPORTANT]
> If the computer that you want to rename is a domain controller, see "Rename a domain controller" (<A href="https://go.microsoft.com/fwlink/?linkid=66828">https://go.microsoft.com/fwlink/?LinkId=66828</A>).

**To modify the recipient policy SMTP address**

1. Start Exchange System Manager.

2. Click **Organization**, click **Recipients**, and then click **Recipient Policies**.

3. Double-click the policy that you want to change.

4. Click the **E-Mail Addresses** tab, and then change the appropriate SMTP address
