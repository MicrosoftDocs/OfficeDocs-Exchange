---
title: 'Create a UM auto attendant: Exchange 2013 Help'
TOCTitle: Create a UM auto attendant
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.custom:
- 'Microsoft.Exchange.Management.SnapIn.Esm.OrganizationConfiguration.UnifiedMessaging.CreateAutoAttendantWizardForm.CreateAutoAttendantWizardPage'
ms.assetid: 773f53fb-d80f-4a79-8bd3-bd753942489f
f1.keywords:
- CSH
mtps_version: v=EXCHG.150
---

# Create a UM auto attendant in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

After you create a Unified Messaging (UM) auto attendant, incoming calls to an external telephone number that a human operator would ordinarily answer are answered by the auto attendant. Unlike with other Unified Messaging components, such as UM dial plans and UM IP gateways, you aren't required to create UM auto attendants. However, auto attendants help internal and external callers locate users or departments that exist in an organization and transfer calls to them.

For additional management tasks related to UM auto attendants, see [UM auto attendant procedures](um-auto-attendant-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 3 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM auto attendants" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to create a UM auto attendant

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**, select the UM dial plan for which you want to add an auto attendant, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Auto Attendants**, click **New** ![Add Icon](images/ITPro_EAC_AddIcon.gif).

3. On the **New UM auto attendant** page, enter the following information:

   - **Name**: Use this box to create the display name for the UM auto attendant. A UM auto attendant name is required and must be unique. However, it's used only for display purposes in the EAC and the Shell.

     If you have to change the display name of the auto attendant after it's created, you must first delete the existing UM auto attendant and then create another auto attendant that has the appropriate name. If your organization uses multiple UM auto attendants, we recommend that you use meaningful names for your UM auto attendants. The maximum length of a UM auto attendant name is 64 characters, and it can include spaces.

     Although you can name a new UM auto attendant to include spaces, if you integrate Unified Messaging with Office Communications Server 2007 R2 or Microsoft Lync Server, the name of the auto attendant can't include spaces. Therefore, if you created an auto attendant with spaces in the display name, and you're integrating with Office Communications Server 2007 R2 or Lync Server, you must first delete that auto attendant and then create another auto attendant that doesn't include spaces in the display name.

   - **Create this auto attendant as enabled**: Select this check box to enable the auto attendant to answer incoming calls when you complete the New UM Auto Attendant Wizard. By default, a new auto attendant is created as disabled.

     If you decide to create the UM auto attendant as disabled, you can use the EAC or the Shell to enable the auto attendant after you finish the wizard.

   - **Set the auto attendant to respond to voice commands**: Select this check box to speech-enable the UM auto attendant. If the auto attendant is speech-enabled, callers can respond to the system or custom prompts used by the UM auto attendant using touchtone or voice inputs. By default, the auto attendant won't be speech-enabled when it's created.

     For callers to use a speech-enabled auto attendant, you must install the appropriate UM language pack that contains Automatic Speech Recognition (ASR) support and configure the properties of the auto attendant to use this language.

   - **Access numbers**: Use this box to enter the extension numbers or telephone numbers that callers will use to reach the auto attendant. Type an extension number or telephone number in the box, and then click **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif) to add the number to the list. The number of digits in the extension number or telephone number that you provide doesn't have to match the number of digits for an extension number configured on the associated UM dial plan. This is because direct calls are allowed to UM auto attendants.

     The number of extension numbers or telephone numbers entered is unlimited. However, you may create the new auto attendant without an extension number listed. An extension number or telephone number isn't required.

     You can edit or remove an existing extension number or telephone number. To edit an existing extension number or telephone number, click **Edit** ![Add Icon](images/ITPro_EAC_AddIcon.gif). To remove an existing extension number or telephone number from the list, click **Remove** ![Remove icon](images/ITPro_EAC_RemoveIcon.gif).

4. Click **Save**.

## Use the Shell to create a UM auto attendant

This example creates a UM auto attendant named `MyUMAutoAttendant` that can accept incoming calls but isn't speech-enabled.

```powershell
New-UMAutoAttendant -Name MyUMAutoAttendant -UMDialPlan MyUMDialPlan -PilotIdentifierList 55000 -Enabled $false
```

This example creates a speech-enabled UM auto attendant named `MyUMAutoAttendant`.

```powershell
New-UMAutoAttendant -Name MyUMAutoAttendant -UMDialPlan MyUMDialPlan -PilotIdentifierList 56000,56100 -SpeechEnabled $true
```
