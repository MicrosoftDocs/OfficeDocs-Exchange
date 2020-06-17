---
localization_priority: Normal
description: In your Exchange Online organization, you may need to restrict access to specific recipients. The most common scenario is the need to control messages sent to large distribution groups. Depending on your organization's requirements, you may also need to control the messages sent to executive mailboxes or partner contacts. You can use moderated recipients to accomplish these tasks. When you configure a recipient for moderation, all messages sent to that recipient are subject to approval by the designated moderators.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: f0c3db25-653f-4252-acb1-2b5ba940ab80
ms.reviewer: 
f1.keywords:
- NOCSH
title: Configure a moderated recipient in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Configure a moderated recipient in Exchange Online

In your Exchange Online organization, you may need to restrict access to specific recipients. The most common scenario is the need to control messages sent to large distribution groups. Depending on your organization's requirements, you may also need to control the messages sent to executive mailboxes or partner contacts. You can use moderated recipients to accomplish these tasks. When you configure a recipient for moderation, all messages sent to that recipient are subject to approval by the designated moderators.

## What do you need to know before you begin?

- Estimated time to complete: 15 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the"Moderated Transport" entry in the [Feature permissions in Exchange Online](../permissions-exo/feature-permissions.md) topic.

- You can use the Exchange admin center (EAC) to configure a distribution group for moderation. All other recipient types can only be configured for moderation using PowerShell. To learn how to use Windows PowerShell to connect to Exchange Online, see [Connect to Exchange Online PowerShell](https://go.microsoft.com/fwlink/p/?linkid=396554).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to configure a moderated distribution group
<a name="EMCtoConfigureModeration"> </a>

This example configures the following moderation settings for the distribution group named All Employees:

- Enable moderation for the distribution group.

- Designate David Hamilton and Yossi Ran as moderators.

- Allow the members of the distribution group named HR to bypass moderation.

- Notify internal senders if their message to the distribution group is rejected, but do not send any notifications to external senders.

To accomplish the tasks in this example scenario, perform the following procedure:

1. In the EAC, navigate to **Recipients** \> **Groups**.

2. In the result pane, select the **All employees** distribution group and click **Edit** ![Edit icon](../media/ITPro_EAC_EditIcon.gif).

3. On the properties page, click **Message approval**, and complete the following:

  1. Select the **Messages sent to this group have to be approved by a moderator** check box.

  2. In the **Group moderators** list, click **Add** ![Add Icon](../media/ITPro_EAC_AddIcon.gif).

  3. In the **Select group moderators** dialog, find and select David Hamilton, click **Add**, find and select Yossi Ran, and click **Add**. When you are finished, click **OK**.

  4. In the **Senders who don't require message approval** list, click **Add** ![Add Icon](../media/ITPro_EAC_AddIcon.gif).

  5. In the **Select senders** dialog, find and select HR from the list and click **Add**. When you are finished, click **OK**.

  6. In **Select moderation notifications**, select **Notify all senders when their messages aren't approved**.

4. Click **Save**.

## Use Exchange Online PowerShell to configure a moderated recipient
<a name="EMCtoConfigureModeration"> </a>

Run the following command:

```PowerShell
Set-<RecipientType> <Identity> -ModerationEnabled $true -ModeratedBy <recipient1,recipient2...> -ByPassModerationFromSendersOrMembers <recipient1,recipient2...> -SendModerationNotifications <Never | Always | Internal>
```

This example configures the following moderation settings for the distribution group named All Employees:

- Enable moderation for the distribution group.

- Designate David Hamilton and Yossi Ran as moderators.

- Allow the members of the distribution group named HR to bypass moderation.

- Notify internal senders if their message to the distribution group is rejected, but do not send any notifications to external senders.

To accomplish the tasks in this example scenario, run the following command:

```PowerShell
Set-DistributionGroup "All Employees" -ModerationEnabled $true -ModeratedBy "David Hamilton","Yossi Ran" -ByPassModerationFromSendersOrMembers HR -SendModerationNotifications Internal
```

To add or remove users from the list of moderators or recipients who bypass moderation without affecting other entries, use the following syntax:

```PowerShell
Set-<RecipientType> <Identity> -ModeratedBy @{Add="<recipient1>","<recipient2>"...; Remove="<recipient1>","<recipient2>"...} -ByPassModerationFromSendersOrMembers @{Add="<recipient1>","<recipient2>"...; Remove="<recipient1>","<recipient2>"...}
```

This example configures the following moderation settings for the distribution group named All Employees:

- Add the user chris@contoso.com to the list of existing moderators.

- Remove the user michelle@contoso.com from the list of existing senders who bypass moderation.

```PowerShell
Set-DistributionGroup "All Employees" -ModeratedBy @{Add="chris@contoso.com"} -ByPassModerationFromSendersOrMembers @{Remove="michelle@contoso.com"
```

## How do you know this worked?

To verify that you have successfully configured a recipient for moderation, do the following:

1. Send a test message to the moderated recipient.

2. Verify the designated moderators receive notification.

3. Verify the recipients who bypass moderation receive the message directly.
