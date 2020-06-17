---
title: 'Manage a UM auto attendant: Exchange 2013 Help'
TOCTitle: Manage a UM auto attendant
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.custom:
- 'Microsoft.Exchange.Management.SnapIn.Esm.OrganizationConfiguration.UnifiedMessaging.AutoAttendantGeneralPropertyPage'
ms.assetid: 4809ff56-ae34-4ce6-8e39-9193311c3f83
f1.keywords:
- CSH
mtps_version: v=EXCHG.150
---

# Manage a UM auto attendant in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

After you create a Unified Messaging (UM) auto attendant, you can view or configure a variety of settings. For example, you can add, remove, and edit extension numbers associated with the auto attendant. You can also enable or disable Automatic Speech Recognition (ASR) for the auto attendant and change the greetings used for business and non-business hours.

For additional management tasks related to UM auto attendants, see [UM auto attendant procedures](um-auto-attendant-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM auto attendants" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM auto attendant has been created. For detailed steps, see [Create a UM auto attendant](create-a-um-auto-attendant-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to view or configure UM auto attendant settings

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Auto Attendants**, select the UM auto attendant you want to view or configure, and then on the toolbar, click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the **UM Auto Attendant** page, click **General** to view display-only information about the UM auto attendant and to perform management tasks on the UM auto attendant, as follows:

   - **UM dial plan**: This box displays the UM dial plan associated with the auto attendant. After you create an auto attendant, the dial plan associated with the auto attendant can't be changed. If you need to associate an auto attendant with a different dial plan, you must delete the dial plan and then associate the auto attendant with the correct dial plan after you re-create it.

   - **Name**: This box shows the name that was assigned to the auto attendant when it was created. This is the name that will appear in the EAC.

   - **Status**: This box shows whether the UM auto attendant is enabled or disabled. To enable or disable the auto attendant, close the **UM Auto Attendant** page and use the toolbar under **UM Auto Attendants** on the **UM Dial Plan** page.

   - **Access numbers**: Use this box to enter an extension number or access number that leads callers to the auto attendant. By default, no extension or access numbers are configured when you create an auto attendant.

     The number of digits in the extension numbers or access numbers you provide must match the number of digits for an extension number configured on the UM dial plan associated with the UM auto attendant. You can also add a Session Initiation Protocol (SIP) address to this box. A SIP address is used by some IP Private Branch eXchanges (PBXs), SIP-enabled PBXs, and Microsoft Office Communications Server 2007 R2 or Microsoft Lync Server.

    You can create a new auto attendant without listing an extension number or access number. To add an extension number, type the number in this box, and then click **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif). You can associate more than one number with an auto attendant. You can also edit or remove an existing access number. To edit an existing number, select it and click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif). To remove an existing extension number from the list, select it and click **Remove** ![Remove icon](images/ITPro_EAC_RemoveIcon.gif).

   - **Set the auto attendant to respond to voice commands**: Select this check box to enable callers to respond verbally to auto attendant prompts to navigate the menu system. By default, when an auto attendant is created, it isn't speech-enabled.

     If you decide to create the UM auto attendant but not to speech-enable it, you can use the EAC or the Shell to speech-enable it after it is created.

   - **Use this auto attendant when voice commands don't work correctly**: Click **Browse** to select the auto attendant that you want to use in the case that voice commands don't work. This is also referred to as a DTMF fallback auto attendant. A DTMF fallback auto attendant can be used only if the **Set the auto attendant to respond to voice commands don't work correctly** option is selected. You must first create a DTMF fallback auto attendant, and then click **Browse** to locate the appropriate DTMF auto attendant.

      A DTMF fallback auto attendant is used when the UM speech-enabled auto attendant can't understand or recognize the speech inputs from the caller. If the DTMF auto attendant is used, the caller is required to use DTMF inputs to navigate the menu system, spell a user's name, or use a custom menu prompt. A caller won't be able to use voice commands to navigate this auto attendant.

      If you don't configure a DTMF fallback auto attendant, we recommend that you configure an operator extension number on the auto attendant. If you don't configure an operator extension number, when callers use a speech-enabled auto attendant and the system doesn't recognize their voice inputs, they won't be able to navigate the system or be transferred to an operator for help.

      Although not required, we recommend that you configure the DTMF fallback auto attendant to have the same configuration as the speech-enabled auto attendant. The DTMF fallback auto attendant shouldn't be speech-enabled.

   - **Language for automated voice interface**: Use this list to select the language that callers hear when they reach the auto attendant. The default language is determined when you install Microsoft Exchange. For on-premises and hybrid deployments, by default, U.S. English is used because the auto attendant uses the language setting on the UM dial plan. To have other language options available, you must install the UM language packs for the languages you want to include. For more information about how to install a UM language pack, see [Install a UM language pack in Exchange Server](install-a-um-language-pack-exchange-2013-help.md).

     Although you can select a language other than the language selected on the UM dial plan associated with the auto attendant, we recommend that the language settings on the dial plan and the auto attendant match. If language settings don't match, when callers call an extension number defined on the dial plan, they will be presented with prompts in one language, and when they dial an extension number associated with an auto attendant, they will be presented with prompts in a different language.

   - **Business name**: Use this box to enter the name of the business. By default, no business name is entered. If you enter a business name in this box, a prompt with the business name will be played to callers instead of the default greeting.

   - **Business location**: Use this box to enter the location of the business. By default, no business location is entered. If you enter the location of the business in this box, the business location will be played for callers.

4. Use **Greetings** on the auto attendant to manage recorded greetings. You can select default greetings or previously recorded custom greetings for business hours and non-business hours. You can configure the following:

   - **Business hours greeting**: This is the initial greeting that is played when a caller calls the auto attendant during your organization's business hours. By default, business hours are from 12:00 A.M. to 12:00 A.M. and no non-business hours are set. If you don't specify a custom greeting, a system prompt that says, "Welcome to the Exchange auto attendant" is played for callers. The business and non-business hours are configured on the auto attendant **Business hours**.

     You may want to customize this greeting to represent your company, for example, "Thank you for calling Woodgrove Bank." You can configure a customized business hours greeting by clicking **Change** to select a previously recorded custom greeting file. The custom greeting must already have been recorded as a .wav or .wma file.

   - **Non-business hours greeting**: This is the initial greeting played when a caller calls the auto attendant during your organization's non-business hours. By default, no non-business hours are configured. Therefore, there is no default non-business hours greeting. You can configure the business and non-business hours on the auto attendant **Business hours**.

    You may want to customize this greeting to represent your company, for example, "Thank you for calling Woodgrove Bank but we are now closed." or "You have reached Contoso, Ltd. after business hours. Our business hours are from 8:00 A.M. until 5:00 P.M., Monday through Friday." You can configure a customized non-business hours greeting by clicking **Change** to select a previously recorded custom greeting file. The custom greeting must already have been recorded as a .wav or .wma file.

   - **Informational announcement**: When enabled, this optional recording plays immediately after the business or non-business hours greeting. An informational announcement may state the organization's hours of operation, for example, "Our business hours are 8:30 A.M. to 5:30 P.M., Monday through Friday and 8:30 A.M. to 1:00 P.M. on Saturday." An informational announcement can also provide information required for compliance with company policy, for example, "Calls may be monitored for training purposes." If it's important that callers hear the whole informational announcement, it can be marked as uninterruptible, requiring the caller to listen to the whole announcement.

     By default, there's no informational announcement configured on UM dial plans or auto attendants. If you enable an informational announcement and use a custom audio file specific to your organization, the **Allow announcement to be interrupted** option will be made available. The recordings must already have been recorded as .wav or .wma files. Click **Change** to locate a custom informational announcement file previously recorded.

   - **Allow announcement to be interrupted**: Select this check box to enable the caller to interrupt the informational announcement. This should be enabled if you have long informational announcements. Callers may become frustrated if the informational announcement is long and they can't interrupt it to access the options provided by the auto attendant.

5. Use **Business hours**: to determine the organization's open business hours. During business hours, callers hear the default business hours greeting or a customized greeting, and the business hours main menu prompt if the appropriate business hours key mappings are configured on **Menu navigation**. You can configure the following:

   - **Time zone**: Use this list to select your time zone. Consider whether the dial plan associated with the auto attendant covers more than one time zone when you set your schedule.

     For on-premises and hybrid deployments, by default, the time zone is configured using the local server's system time when the Mailbox server that is running the Microsoft Exchange Unified Messaging service was installed.

   - **Business hours**: Click **Configure business hours**, and then, on the **Configure Business Hours**: page, use the grid to configure your organization's business hours.

   - **Holiday schedule**: Use this to define days, from 00:00 through 23:59 (12:00 A.M. through 11:59 P.M.), on which your organization will be closed for a holiday. Callers who reach the auto attendant during the times that you specify on the **New holiday** page hear a custom holiday greeting audio file that you define. When you configure the holiday schedule, you must define the holiday name, the audio file for the recorded holiday greeting, and the **Start date** and **End date**. The greetings must already have been recorded as .wav or .wma files.

6. Use **Menu navigation** to specify the menu options that are offered to callers during business and non-business hours. If you want to enable menu navigation, you must do it separately for business and non-business hours. For example, if you want to enable business hours navigation, you must add a menu prompt custom audio recording, select the **Enable business hours menu navigation** check box, click **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif), and then set the options on the **New menu navigation entry** page.

   - **Business hours menu navigation**: This is the list of options that callers hear during the business hours that are defined on the **Business hours** page. For example, "For technical support, press or say 1. For corporate offices and administration, press or say 2. For sales, press or say 3."

     To enable business hours menu navigation, you must perform the following steps:

     1. **Menu prompt**: Use this to specify a custom menu prompt audio file. To use a custom or previously recorded business hours menu prompt, click **Change**, and then click **Browse** to locate the menu prompt recording.

     2. **Enable business hours menu navigation**: Select this check box to enable options for menu navigation that will be used during business hours. When you enable business hours menu navigation, you can add new menu navigation entries for business hours.

     3. Click **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif) to create a new menu navigation entry. On the **New menu navigation entry** page, use the following options to create a new menu navigation entry:

        - **Prompt**: Use this box to type the name of the new navigation menu. The navigation menu name is used for display purposes only. This is a required field.

         Because you may want to specify multiple new navigation menus, we recommend that you use meaningful names for your key mappings. The maximum length of the name for the key mapping is 64 characters, and it can include spaces. However, it can't include any of the following characters: `" / \ [ ] : ; | = , + * ? < >`.

        - **When this key is pressed**: Use this list to enable key mapping. The key mapping is the number key that a caller presses to have the auto attendant perform a specific operation, for example, forwarding the caller to another auto attendant or to an operator. By default, no entries are defined.

          Use the drop down list to select the numeric key (from 1 through 9) that the caller must press. Zero (0) is reserved for the auto attendant operator.

          If you select **Time Out** from the drop down list, it enables callers to be transferred to an extension number or to another auto attendant if they don't press a key on the telephone keypad. For example, "Please stay on the line and your call will be answered by the next available representative." The default setting is 5 seconds. If you enable this option, a blank key mapping will be created.

   - **Play the following audio file**: Use this option to select a previously recorded audio file for callers. Click **Change**, and then click **Browse** to locate the audio file. If you leave the audio file as the default \<None\>, the Unified Messaging TTS (Text to Speech) engine will synthesize a business hours main menu prompt. Alternatively, you can create a customized audio file that can be used for the business hours main menu prompt for a speech-enabled auto attendant. For example, it might say, "To leave a voice message for sales, say 1. To leave a voice message for technical support, say 2. To leave a voice message for administration, say 3."

   - **Perform this additional action**: Select one of the following options to define the action that you want the auto attendant to perform for the caller:

     - **None**: If you don't want the auto attendant to transfer the call to an extension or to another auto attendant, or leave a message for a user, use this option.

     - **Transfer to this extension**: Select this option to enable calls to be transferred to an extension number. If you enable this option, use the box to type the extension where the call will be transferred. This field allows only numeric characters. It can't include any of the following characters: `" / \ [ ] : ; | = , + * ? < >`.

     - **Transfer to this UM auto attendant**: Select this option to transfer the call to an auto attendant. Click **Browse** to locate the auto attendant that you want to use. Before you enable this option, you must first create and configure the auto attendant. This option is used when you create a parent/child structure of UM auto attendants.

     - **Leave a voice message for this user**: Select this option to enable a caller to leave a voice mail message for a user that's on the same dial plan as the UM auto attendant that you're configuring. When a caller chooses this option from an auto attendant menu, they'll be prompted to leave a voice message for the user that was selected. Click **Browse** to locate the UM-enabled user.

     - **Announce business location**: Select this option to enable a caller to choose an auto attendant menu option and hear the location of the business that's configured on the UM auto attendant. To enable this to work correctly, you must first enter the business location in the **Business location** box on the **General** page on the UM auto attendant.

     - **Announce business hours**: Select this option to enable a caller to choose an auto attendant menu option and hear the hours of operation for the business that's configured on the UM auto attendant. To enable this to work correctly, you must first configure the business hours on the **Business hours** page on the UM auto attendant.

   - **Non-Business hours menu navigation**: This is the list of options callers hear during the non-business hours that are defined on the **Business hours** page. For example, "Your call is very important to us. However, you have reached Woodgrove Bank after normal business hours. If you want to leave a message, please press or say 1 and we will return your call as soon as possible."

     To enable non-business hours menu navigation, you must perform the following steps:

     1. **Menu prompt**: Use this to specify a custom menu prompt audio file. To use a custom or previously recorded non-business hours menu prompt, click **Browse**.

     2. **Enable non-business hours menu navigation**: Select this check box to enable options for menu navigation that will be used during non-business hours. When you enable non-business hours menu navigation, you can add new menu navigation entries for non-business hours.

     3. Click **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif) to create a new menu navigation entry. On the **New menu navigation entry** page, use the following options to create a new menu navigation entry:

        - **Prompt**: Use this box to type the name of the new navigation menu. The navigation menu name is used for display purposes only. This is a required field.

          Because you may want to specify multiple new navigation menus, we recommend that you use meaningful names for your key mappings. The maximum length of the name for the key mapping is 64 characters, and it can include spaces. However, it can't include any of the following characters: `" / \ [ ] : ; | = , + * ? < >`.

        - **When this key is pressed**: Use this list to enable key mapping. The key mapping is the number key that a caller presses to have the auto attendant perform a specific operation, for example, forwarding the caller to another auto attendant or to an operator. By default, no entries are defined.

          Use the drop down list to select the numeric key (from 1 through 9) that the caller must press. Zero (0) is reserved for the auto attendant operator.

          If you select **Time Out** from the drop down list, it enables callers to be transferred to an extension number or to another auto attendant if they don't press a key on the telephone keypad. For example, "Please stay on the line and your call will be answered by the next available representative." The default setting is 5 seconds. If you enable this option, a blank key mapping will be created.

   - **Play the following audio file**: Use this option to select a previously recorded audio file for callers. Click **Change**, and then click **Browse** to locate the audio file. If you leave the audio file as the default \<None\>, the Unified Messaging TTS (Text to Speech) engine will synthesize a non-business hours main menu prompt. Alternatively, you can create a customized audio file that can be used for the non-business hours main menu prompt for a speech-enabled auto attendant that would say, for example, "You have reached Contoso during non-business hours. To leave a voice message for sales, say 1. To leave a voice message for technical support, say 2. To leave a voice message for administration, say 3. To reach an after hours operator, press zero."

   - **Perform this additional action**: Select one of the following options to define the action that you want the auto attendant to perform for the caller:

     - **None**: If you don't want the auto attendant to transfer the call to an extension or to another auto attendant, or leave a message for a user, use this option.

     - **Transfer to this extension**: Select this option to enable calls to be transferred to an extension number. If you enable this option, use the box to type the extension number where the call will be transferred. This field allows only numeric characters. It can't include any of the following characters: `" / \ [ ] : ; | = , + * ? < >`.

     - **Transfer to this UM auto attendant**: Select this option to transfer the call to an existing auto attendant. Click **Browse** to locate the auto attendant that you want to use. Before you enable this option, you must first create and configure the auto attendant. This option is used when you create a parent/child structure of UM auto attendants.

     - **Leave a voice message for this user**: Select this option to enable a caller to leave a voice mail message for a user that's on the same dial plan as the UM auto attendant that you're configuring. When a caller chooses this option from an auto attendant menu, they'll be prompted to leave a voice message for the user that was selected. Click **Browse** to locate the UM-enabled user.

     - **Announce business location**: Select this option to enable a caller to choose an auto attendant menu option and hear the location of the business that's configured on the UM auto attendant. To enable this to work correctly, you must first enter the business location in the **Business location** box on the **General** page on the UM auto attendant.

     - **Announce business hours**: Select this option to enable a caller to choose an auto attendant menu option and hear the hours of operation for the business that's configured on the UM auto attendant. To enable this to work correctly, you must first configure the business hours on the **Business hours** page on the UM auto attendant.

7. Use **Address book and operator access**: to define the features available to callers who dial in to the UM auto attendant. You can configure auto attendant features such as the language used when callers call in to the auto attendant and the ability for callers to transfer to an operator's extension number. You can configure the following:

   - **Options for contacting users**: Use these options to determine how callers can contact users with voice mail when they call into a UM auto attendant

   - **Allow callers to dial users**: Select this check box to enable callers to transfer calls to users. By default, this option is enabled, and lets users who are associated with the dial plan transfer calls to users in the same UM dial plan. After you select this check box, you can set the group of users to whom callers can transfer by selecting the appropriate option under the **Options for searching the address book** section on this page.

     If you disable this option and disable the **Allow callers to leave voice messages for users** option, the options under **Options for searching the address book** are also disabled.

   - **Allow callers to leave voice messages for users**: Select this check box to enable callers to send voice messages to users. By default, this option is enabled, and lets users who are associated with the dial plan send voice messages to users in the same UM dial plan. After you select this check box, you can set the group of users to whom callers can send voice messages by selecting the appropriate option under the **Options for searching the address book** section on this page.

     If you disable this option and disable the **Allow callers to dial users** option, the options under **Options for searching the address book** are also disabled.

     If you disable this option, the auto attendant won't invite callers to send a voice message during a system prompt.

   - **Options for searching the address book**: Use these options to determine a grouping of users. By default, **Allow callers to search for user by name or alias** is selected, along with the **In this dial plan only** option. However, you can change the grouping of users to allow callers to transfer calls or send voice messages to users who are located in the global address list (GAL) for an organization. You can choose from the following:

   - **Allow callers to search for users by name or alias**: By default, this option is selected. It allows callers that call into this auto attendant to do a directory search for users by name or by their alias. An alias is assigned to a user when a mailbox is created for them. The alias is the first part of an SMTP address, for example, tonysmith@contoso.com. The SMTP address is tonysmith@contoso.com, while the alias is tonysmith. Choosing this option only affects callers that use this auto attendant and not those who use Outlook Voice Access.

   - **In this dial plan only**: Select this option to allow callers who connect to the UM auto attendant to locate and contact users who are in the same dial plan that is associated with this UM auto attendant. By default, this option is enabled on the dial plan and on the auto attendant. This means that both Outlook Voice Access users and callers into the auto attendant are able to search for users within the same dial plan.

   - **In the entire organization**: Select this option to allow callers who call into this UM auto attendant to search for and contact anyone listed in the GAL for the organization. This includes not only UM-enabled users but all users who are mailbox-enabled. This option allows callers to contact users in multiple dial plans. It isn't enabled by default. This setting is also available on a dial plan for Outlook Voice Access users.

   - **Information to include for similar names**: Use this drop-down list to select the option used for the UM auto attendant when users have the same or similar names. This setting is used when two or more users who have the same name exist in the directory. This is also called a matched name or disambiguation field. You can configure this setting, or you can leave the default setting on the auto attendant. By default, the auto attendant will inherit this setting from the setting on the dial plan that is linked to the auto attendant. The following is an example of a speech-enabled auto attendant:

     1. System: "Welcome to Contoso. If you know the name of the person you are calling, please tell me their name at any time."

     2. Caller says "Tony Smith."

     3. There are multiple people with this name. Please select from one of the options: For Tony Smith, research, press 1. For Tony Smith, administration, press 2. For Tony Smith, technical support, press 3."

     4. Caller presses the appropriate key on the key pad and the call is transferred to the user.

        > [!NOTE]
        > On a non-speech-enabled auto attendant, the system will tell the caller to use the key pad to input the user's name (last name first) and then search for the user. If there are multiple people in the directory with the same name, the caller is instructed to press the appropriate key to be transferred to the user. You could optionally create a DTMF fallback auto attendant that uses only the key pad to enter a name or alias.

        For these settings to be used, you must add the correct information to the user. For example, if you want the auto attendant to use a title for two users with the same name, you must add this information to the user's account. Select one of the following methods that provide more information to help the caller select the correct user in the organization:

        - **Inherit From dial plan**: Select this option to have the auto attendant use the default setting from the dial plan associated with the auto attendant.

        - **Title**: Select this option to have the auto attendant include each user's title when listing matches.

        - **Department**: Select this option to have the auto attendant include each user's department when listing matches.

        - **Location**: Select this option to have the auto attendant include each user's location when listing matches.

        - **None**: Select this option to have no additional information given when listing matches.

        - **Prompt for alias**: Select this option to have the auto attendant prompt the caller for the user's alias.

8. Under **Operator access**, you can specify auto attendant operator settings including the following:

   - **Operator extension**: Use this box to type the extension number used to call an operator. This extension number can connect the caller to a human operator or a UM-enabled mailbox, or can be configured to call an external telephone number. By default, an operator extension isn't included in this box.

   - **Allow transfer to operator during business hours**: Select this check box to enable callers to be transferred to a human operator during business hours by using the extension number that you configure in the **Operator extension** box. By default, this option is disabled.

     It's useful to enable this option so that when a caller is unsuccessful at using the menu prompts or directory search to locate the required person during business hours, the caller can leave a voice message or connect to a human operator. After you enable this option, you can configure the operator extension number on a UM-enabled mailbox that's monitored. The caller can leave a voice message, or a human operator who has the extension number can help the caller.

   - **Allow transfer to operator during non-business hours**: Select this check box to enable callers to be transferred to a human operator after business hours by using the extension number that you configure in the **Operator extension** box. By default, this option is disabled.

     It's useful to enable this option so that when a caller is unsuccessful at using the menu prompts or directory search to locate the required person after business hours, the caller can leave a voice message or connect to a human operator. After you enable this option, you can configure the operator extension number configured on a UM-enabled mailbox that's monitored. The caller can leave a voice message, or a human operator who has the extension number can help the caller.

9. Use **Dialing authorization**: to configure dialing rules for callers who call in to a UM auto attendant. You can use these settings to control the extension numbers that can be reached from an auto attendant or control the telephone numbers that can be dialed by callers that have dialed into the auto attendant. You can configure the following:

   - **Calls in the same UM dial plan**: Select this check box to allow users who call in to an auto attendant to place or transfer calls to an extension number associated with a UM-enabled user who is associated with the same dial plan as the auto attendant. By default, this setting is enabled.

     When you disable this setting, users who call in to an auto attendant can place or transfer calls to users who aren't UM-enabled or to other extension numbers not associated with a UM-enabled user. Users can't transfer calls to UM-enabled users who are associated with the same dial plan as the auto attendant. This is because the **Allow calls to any extension** setting is enabled by default.

   - **Allow calls to any extension**: When this setting is disabled, users who call in to an auto attendant can't place calls to users who aren't UM-enabled or to other extension numbers not associated with a UM-enabled user. However, they can place calls or transfer calls to extension numbers associated with UM-enabled users. This is because the **Calls in the same UM dial plan** setting is enabled by default. The **Allow calls to any extension** setting is enabled by default.

     When this setting is enabled, users who call in to an auto attendant can place calls to users who aren't UM-enabled, to other extension numbers not associated with a UM-enabled user, and to UM-enabled users. This is because the **Calls within the same UM dial plan** setting is enabled by default.

     You can enable this setting in an environment where not all users have been UM-enabled. This setting is also useful when you want to allow users who call in to a telephone number configured on an auto attendant to call extension numbers not associated with a UM-enabled user.

   - **Authorized in-country/region dialing rule groups**: Use this section to add or remove allowed in-country/region dialing rule groups. By default, there are no in-country/region dialing rule groups configured on UM auto attendants.

     In-country/region dialing rule groups are used to allow or restrict the telephone numbers within a country or region that any user who has dialed in to the UM auto attendant can dial. This helps prevent unnecessary or unauthorized telephone calls and charges.

     To add in-country/region dialing rule groups, you must first create the appropriate in-country/region dialing rule groups on the dial plan associated with the UM auto attendant, and then add the appropriate dialing rule group.

     In-country/region dialing rule groups can be used by Unified Messaging to allow or restrict access to telephone numbers within a country or region. This is applied to any user who has called in to an auto attendant. For more information about outdialing, see [Allow users to make calls](allow-users-to-make-calls-exchange-2013-help.md).

   - **Authorized international dialing rule groups**: Use this section to add or remove allowed international dialing rule groups. By default, there are no international dialing rule groups configured on UM auto attendants.

     International dialing rule groups are used to allow or restrict the telephone numbers outside a country or region that any user who has dialed in to the UM auto attendant can dial. This helps prevent unnecessary or unauthorized telephone calls and charges.

     To add international dialing rule groups, you must first create the appropriate international dialing rule groups on the dial plan associated with the UM auto attendant. After you create the required dialing rule groups on the dial plan, you must then add the dialing rule groups to the list of authorized dialing rule groups on the UM auto attendant.

     International dialing rule groups can be used by Unified Messaging to allow or restrict access to telephone numbers outside a country or region. This is applied to any user who has called in to an auto attendant. For more information about outdialing, see [Allow users to make calls](allow-users-to-make-calls-exchange-2013-help.md).

10. Click **OK** to create the new menu navigation.

11. On the **UM Auto Attendant** page, click **Save** to save your changes.

## Use the Shell to configure UM auto attendant properties

This example configures a UM auto attendant named `MySpeechEnabledAA` to fall back to the `MyDTMFAA` auto attendant, sets the operator's extension to 50100, and enables transfers to this extension number after business hours.

```powershell
Set-UMAutoAttendant -Identity MySpeechEnabledAA -DTMFFallbackAutoAttendant MyDTMFAA -OperatorExtension 50100 -AfterHoursTransferToOperatorEnabled $true
```

This example configures a UM auto attendant named `MyUMAutoAttendant` that has: Business hours configured as 10:45 to 13:15 (10:45 A.M. to 1:15 P.M.) on Sunday, 09:00 to 17:00 (9:00 A.M. to 5:00 P.M.) on Monday, and 09:00 to 16:30 (9:00 A.M. to 4:30 P.M.) on Saturday; holiday times and their associated greetings configured as "New Year" on January 2, 2013; and "Building Closed for Construction" configured from April 24 through April 28, 2013.

```powershell
Set-UMAutoAttendant -Identity MyUMAutoAttendant -BusinessHoursSchedule 0.10:45-0.13:15,1.09:00-1.17:00,6.09:00-6.16:30 -HolidaySchedule "New Year,newyrgrt.wav,1/2/2013","Building Closed for Construction,construction.wav,4/24/2013,4/28/2013"
```

## Use the Shell to view UM auto attendant properties

This example returns a formatted list of all UM auto attendants.

```powershell
Get-UMAutoAttendant | Format-List
```

This example displays the properties of a UM auto attendant named MyUMAutoAttendant.

```powershell
Get-UMAutoAttendant -Identity MyUMAutoAttendant
```
