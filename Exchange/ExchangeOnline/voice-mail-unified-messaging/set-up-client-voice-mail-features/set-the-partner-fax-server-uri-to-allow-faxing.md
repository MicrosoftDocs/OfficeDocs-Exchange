---
localization_priority: Normal
description: You can enable and disable inbound faxes for users associated with a Unified Messaging (UM) mailbox policy. By default, when you enable users for UM, users can't receive fax messages until you enable inbound faxing on the UM mailbox policy and specify the URI for the partner fax server. If the URIs are configured on the UM mailbox policy but the option to allow incoming faxes is disabled on the UM dial plan or for an individual user, UM-enabled users linked to the UM mailbox policy still won't be able to receive faxes.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 77a9013b-d76b-4af2-8b2c-cef435cf67af
ms.reviewer: 
f1.keywords:
- NOCSH
title: Set the partner fax server URI to allow faxing in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Set the partner fax server URI to allow faxing in Exchange Online

You can enable and disable inbound faxes for users associated with a Unified Messaging (UM) mailbox policy. By default, when you enable users for UM, users can't receive fax messages until you enable inbound faxing on the UM mailbox policy and specify the URI for the partner fax server. If the URIs are configured on the UM mailbox policy but the option to allow incoming faxes is disabled on the UM dial plan or for an individual user, UM-enabled users linked to the UM mailbox policy still won't be able to receive faxes.

For more information about fax partners, see [Microsoft solution providers](https://www.microsoft.com/solution-providers/).

For additional management tasks related to faxing, see [Faxing procedures](faxing-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](../../voice-mail-unified-messaging/set-up-voice-mail/create-um-mailbox-policy.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to set the fax partner URI

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to modify, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. On the **UM dial plan** page, under **UM Mailbox Policies**, select the policy you want to modify, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **UM mailbox policy** page \> **General**, in the **Partner fax server URI** box, enter the TCP or TLS URI. For example: _sip:faxserver1.contoso.com:5060;transport=tcp_ or _sip:faxserver2.contoso.com:5061;transport=tls_

    > [!NOTE]
    > Although the box can contain more than one fax server URI, only one will be used. If you enter two URIs, only the first will be used.

4. Click **Save** to save your changes.

## Use Exchange Online PowerShell to set the fax partner URI

This example allows users who are linked with the UM mailbox policy `UMDialPlan Default Policy` to use TCP with port 5060 for the partner fax server `faxserver1`.

```PowerShell
Set-UMMailboxPolicy "UMDialPlan Default Policy" -FaxServerURI sip:faxserver1.contoso.com:5060;transport=tcp
```

This example allows users who are linked with the UM mailbox policy `UMDialPlan Default Policy` to use TLS with port 5061 for the partner fax server `faxserver2`.

```PowerShell
Set-UMMailboxPolicy "UMDialPlan Default Policy" -FaxServerURI sip:faxserver2.contoso.com:5061;transport=tls
```
