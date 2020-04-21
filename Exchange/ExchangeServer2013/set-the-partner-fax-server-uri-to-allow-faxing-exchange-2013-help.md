---
title: 'Set the partner fax server URI to allow faxing: Exchange 2013 Help'
TOCTitle: Set the partner fax server URI to allow faxing
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 77a9013b-d76b-4af2-8b2c-cef435cf67af
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Set the partner fax server URI to allow faxing in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can enable and disable inbound faxes for users associated with a Unified Messaging (UM) mailbox policy. By default, when you enable users for UM, users can't receive fax messages until you enable inbound faxing on the UM mailbox policy and specify the URI for the partner fax server. If the URIs are configured on the UM mailbox policy but the option to allow incoming faxes is disabled on the UM dial plan or for an individual user, UM-enabled users linked to the UM mailbox policy still won't be able to receive faxes.

For additional management tasks related to faxing, see [Faxing procedures](faxing-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM mailbox policies" entry in the [Unified Messaging Permissions](https://technet.microsoft.com/library/d326c3bc-8f33-434a-bf02-a83cc26a5498.aspx) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to set the fax partner URI

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to modify, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the **UM dial plan** page, under **UM Mailbox Policies**, select the policy you want to modify, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the **UM mailbox policy** page \> **General**, in the **Partner fax server URI** box, enter the TCP or TLS URI. For example: _sip:faxserver1.contoso.com:5060;transport=tcp_ or _sip:faxserver2.contoso.com:5061;transport=tls_

    > [!NOTE]
    > Although the box can contain more than one fax server URI, only one will be used. If you enter two URIs, only the first will be used.

4. Click **Save** to save your changes.

## Use the Shell to set the fax partner URI

This example allows users who are linked with the UM mailbox policy `UMDialPlan Default Policy` to use TCP with port 5060 for the partner fax server `faxserver1`.

```powershell
Set-UMMailboxPolicy "UMDialPlan Default Policy" -FaxServerURI sip:faxserver1.contoso.com:5060;transport=tcp
```

This example allows users who are linked with the UM mailbox policy `UMDialPlan Default Policy` to use TLS with port 5061 for the partner fax server `faxserver2`.

```powershell
Set-UMMailboxPolicy "UMDialPlan Default Policy" -FaxServerURI sip:faxserver2.contoso.com:5061;transport=tls
```
