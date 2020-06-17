---
title: 'Create a UM mailbox policy: Exchange 2013 Help'
TOCTitle: Create a UM mailbox policy
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.custom:
- 'Microsoft.Exchange.Management.SnapIn.Esm.Servers.UnifiedMessaging.CreateUMMailboxPolicyWizardForm.CreateUMMailboxPolicyWizardPage'
ms.assetid: 7f20874b-c46c-4505-9a78-f63eacb578ff
f1.keywords:
- CSH
mtps_version: v=EXCHG.150
---

# Create a UM mailbox policy in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can create a Unified Messaging (UM) mailbox policy to apply a common set of UM policy settings, such as PIN policy settings or dialing restrictions, to a collection of UM-enabled mailboxes. UM mailbox policies link a UM-enabled user with a UM dial plan and apply a common set of policies or security settings to a collection of UM-enabled mailboxes. UM mailbox policies are useful for applying and standardizing UM configuration settings for UM-enabled users.

By default, when a UM dial plan is created, a UM mailbox policy is also created. You may have to create additional UM mailbox policies or modify existing UM mailbox policies after you deploy Unified Messaging in your organization.

For additional management tasks related to UM mailbox policies, see [UM mailbox policy procedures](um-mailbox-policy-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 3 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM mailbox policies" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to create a UM mailbox policy

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to modify, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Mailbox Policies**, click **New** ![Add Icon](images/ITPro_EAC_AddIcon.gif).

3. On the **New UM mailbox policy** page, in the **Name** box, enter the name of the new UM mailbox policy.

    Use this box to specify a unique name for the UM mailbox policy. This is a display name that appears in the EAC. If you need to change the display name of the UM mailbox policy after it's been created, you must first delete the existing UM mailbox policy, and then create another UM mailbox policy that has the appropriate name. You can't delete a UM mailbox policy if any UM-enabled users are associated with it.

    The UM mailbox policy name is required, but it's used for display purposes only. Because your organization may use multiple UM mailbox policies, we recommend that you use meaningful names for your UM mailbox policies. The maximum length of a UM mailbox policy name is 64 characters, and it can include spaces. However, it cannot include any of the following characters: `" / \ [ ] : ; | = , + * ? < >`.

4. Click **Save** to save the new UM mailbox policy. When you save the UM mailbox policy, all of the default settings including PIN policies, voice mail features, and Protected Voice Mail settings are enabled. If you want to customize or change any default settings, use the **Set-UMMailbox** cmdlet to change the settings for the UM mailbox policy you just created.

## Use the Shell to create a UM mailbox policy

This example creates a UM mailbox policy named `MyUMMailboxPolicy` associated with a UM dial plan named `MyUMDialPlan`.

```powershell
New-UMMailboxPolicy -Name MyUMMailboxPolicy -UMDialPlan MyUMDialPlan
```
