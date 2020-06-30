---
title: 'Deploy Exchange 2013 in an Exchange resource forest topology'
TOCTitle: Deploy Exchange 2013 in an Exchange resource forest topology
ms:assetid: 537a7b2b-d002-40a6-84ae-fd02635f9e23
ms:mtpsurl: https://technet.microsoft.com/library/Aa998031(v=EXCHG.150)
ms:contentKeyID: 50406263
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Deploy Exchange 2013 in an Exchange resource forest topology

_**Applies to:** Exchange Server 2013_

This topic explains how to deploy Microsoft Exchange 2013 in an Exchange resource forest topology. An Exchange resource forest is also called a dedicated Exchange forest. This topic assumes that you don't have an existing Exchange 2013 topology.

The following figure shows an Exchange organization with a resource forest.

**Example of an Exchange organization with an Exchange resource forest**

![Complex Exchange organization with resource forest](images/Aa998031.706725cf-e520-4b89-a275-acd8fb58943a(EXCHG.150).gif "Complex Exchange organization with resource forest")

## What do you need to know before you begin?

To perform the following procedure in Exchange 2013, confirm you have the following:

  - You have the following two Active Directory forests:

      - One forest contains the user accounts for your organization. In this procedure, this forest is called the *accounts forest*.

      - One forest does not contain user accounts and does not yet have Exchange installed. In this procedure, this forest is called the *Exchange forest*. You will use the procedure to install Exchange 2013 in this forest.

  - You have correctly configured Domain Name System (DNS) for name resolution across forests in your organization. To check that you have DNS configured correctly, ping each forest from the other forest or forests in your organization. For more information about configuring DNS, see the [DNS Servers Operations Guide](https://go.microsoft.com/fwlink/p/?linkid=282295).

## Deploy Exchange 2013 in an Exchange resource forest topology

1. From a domain controller in the Exchange forest, create a one-way outgoing trust so that the Exchange forest trusts the accounts forest. For detailed steps, see [Create a one-way, outgoing, forest trust for both sides of the trust](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc779840(v=ws.10)).

    > [!NOTE]
    > Although we recommend that you create a forest trust, you can create either a forest trust or an external trust. If you create an external trust, when you create linked mailboxes in Step&nbsp;3, on the <STRONG>Master Account</STRONG> page of the New Mailbox wizard, you must specify a user account that can access the domain controller in the trusted forest. You can't use the credentials with which you are currently logged on. If you create linked mailboxes by using the <STRONG>New-Mailbox</STRONG> cmdlet, you must specify a user account that can access the domain controller in the trusted forest by using the <EM>LinkedCredential</EM> parameter.

2. In the Exchange forest, install Exchange 2013. Install Exchange the same way that you would in a single forest scenario. For detailed steps about how to install Exchange 2013, see one of the following topics:

      - [Deploy a new installation of Exchange 2013](deploy-a-new-installation-of-exchange-2013-exchange-2013-help.md)

      - [Install Exchange 2013 using the Setup wizard](install-exchange-2013-using-the-setup-wizard-exchange-2013-help.md)

3. In the Exchange forest, for each user in the accounts forest that will have a mailbox in the Exchange forest, create a mailbox that is associated with an external account. For detailed steps, see [Manage linked mailboxes](manage-linked-mailboxes-exchange-2013-help.md).
