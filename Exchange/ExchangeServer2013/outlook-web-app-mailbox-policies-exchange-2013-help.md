---
title: 'Outlook Web App mailbox policies: Exchange 2013 Help'
TOCTitle: Outlook Web App mailbox policies
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 213b8b7a-1c29-49ee-8c98-d0364ddf4f9d
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Outlook Web App mailbox policies in Exchange 2013

_**Applies to:** Exchange Server 2013_

Use Microsoft Outlook Web App mailbox policies to create organization-level policies to manage access to features in Outlook Web App.

## Outlook Web App mailbox policies
<a name="OWA"> </a>

In Exchange 2013, you can create multiple Outlook Web App mailbox policies and apply them to individual mailboxes. When an Outlook Web App mailbox policy is applied to a mailbox, it will override the settings of the virtual directory.

Outlook Web App features can also be managed by configuring the Outlook Web App virtual directories. Virtual directory settings will be used for any mailbox that a mailbox policy hasn't been applied to.

## Creating or deleting Outlook Web App mailbox policies
<a name="Create"> </a>

A default Outlook Web App mailbox policy is created automatically when Exchange is installed. By default, all options are enabled on the default Outlook Web App mailbox policy. You can create as many Outlook Web App mailbox policies as necessary to meet the needs of your organization.

> [!NOTE]
> The default Outlook Web App mailbox policy isn't automatically applied to any mailboxes.

For information about creating or removing mailbox policies, see [Create an Outlook Web App mailbox policy](create-outlook-web-app-mailbox-policy-exchange-2013-help.md) and [Remove an Outlook Web App mailbox policy from Exchange](remove-outlook-web-app-mailbox-policy-exchange-2013-help.md).

## Configuring Outlook Web App mailbox policies
<a name="Configuring"> </a>

The default Outlook Web App mailbox policy has all options enabled by default. For information about configuring Outlook Web App mailbox policies, see [View or configure Outlook Web App mailbox policy properties](configure-outlook-web-app-mailbox-policy-properties-exchange-2013-help.md).

## Applying Outlook Web App mailbox policies
<a name="Applying"> </a>

Only one Outlook Web App mailbox policy can be applied to a mailbox.

If there's no Outlook Web App mailbox policy applied to a mailbox, the settings defined on the virtual directory will be applied.

An Outlook Web App mailbox policy can be applied to a mailbox by using the Exchange admin center (EAC) to modify an existing mailbox, or by using the Shell and the [Set-CASMailbox](https://docs.microsoft.com/powershell/module/exchange/set-casmailbox) cmdlet to apply a mailbox policy. For more information, see [Apply or remove an Outlook Web App mailbox policy on a mailbox](apply-or-remove-outlook-web-app-mailbox-policy-exchange-2013-help.md).
