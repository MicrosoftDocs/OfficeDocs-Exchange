---
title: 'DTMF interface: Exchange 2013 Help'
TOCTitle: DTMF interface
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 2c7c9d8a-ed12-4dcf-a5b7-3cea0e785e49
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# DTMF interface in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

In Unified Messaging (UM), callers can use dual tone multi-frequency (DTMF), also referred to as touchtone, and voice inputs to interact with the system. The methods that callers can use depend on how the UM dial plans and auto attendants are configured.

The DTMF interface enables callers to use the telephone keypad to locate users and navigate the UM voice mail menu system when they call an Outlook Voice Access number configured on a dial plan or when they call a telephone number configured on an auto attendant. This topic discusses the DTMF interface and how it's used by callers to locate users and to navigate the UM voice mail menu system.

## DTMF overview
<a name="dtmfoverview"> </a>

DTMF requires a caller to press a key on the telephone keypad that corresponds to a Unified Messaging menu option or to input a user's name or email alias by using the letters on the keys to spell the name or alias. Callers might use DTMF because Automatic Speech Recognition (ASR) hasn't been enabled or because they tried to use voice commands and failed. In either case, DTMF inputs are used to navigate menus and search for users.

By default, in UM, DTMF inputs are used on dial plans and are the default caller interface for UM auto attendants.

Callers can use DTMF inputs for:

- Dial plan dial-in access by using Outlook Voice Access.

- Dial plan directory lookups and searches to locate users.

- Auto attendants that aren't speech-enabled.

- Auto attendants that are speech-enabled that do or don't have a DTMF fallback auto attendant configured.

- DTMF fallback auto attendants (not speech-enabled).

## UM dial plans and dial by name
<a name="dialplansanddialbyname"> </a>

When you create a UM dial plan, you can configure the primary and secondary input method that callers will use to look up names when they search for a user or want to contact a user. These settings are located on the dial plan's **Settings** page and are called **Primary way of searching for names** and **Secondary way of searching for names**. The following options are available for both the primary and secondary ways of searching for names:

- Last First

- First Last

- SMTP address

Additionally, **None** is an available option for the secondary way of searching for names.

By default, **Last First** is selected as the primary way of searching for names and **SMTP address** is selected as the secondary way of searching for names Therefore, when a caller dials in to an Outlook Voice Access number configured on the UM dial plan, the dial plan's welcome message is played and the operator says something like, "Welcome to Contoso Outlook Voice Access. To access your mailbox, enter your extension. To contact someone, press the pound key." After the caller presses the # key, the system responds with "Spell the name of the person you are calling, last name first, or to spell their email alias, press the pound key twice." In this scenario, depending on how your dial plan is configured, the system then prompts the caller to enter the user's last name and then the user's first name (Last First) or to spell the email alias, excluding the domain name. For example, if the user's email alias is tsmith@contoso.com, the caller would enter tsmith.

If you want to change this configuration because the default setting doesn't meet your needs, you can change it to enable callers to enter the user's email alias first or the user's first name followed by the last name. In this case, you would configure the **Primary way of searching for names** with the **SMTP address** setting and configure the **Secondary way of searching for names** with the **First Last** setting. The settings for the dial by name methods will also apply to any UM auto attendants that are associated with the dial plan. For callers to be able to enter the name of the user by using DTMF inputs or the keys on the telephone keypad, a DTMF map and values for the user must exist within your organization's directory.

For more information about how to change the dial by name primary and secondary methods on a UM dial plan, see [Configure the primary way for Outlook Voice Access users to search](configure-primary-search-method-exchange-2013-help.md) and [Configure the secondary way for Outlook Voice Access users to search](configure-secondary-search-method-exchange-2013-help.md).

## DTMF maps
<a name="dtmfmaps"> </a>

In an Exchange organization, an attribute named **msExchUMDtmfMap** is associated with each user created in the directory. Unified Messaging uses this attribute to map the user's first name, last name, and email alias to a set of numbers. This mapping is referred to as a DTMF map. A DTMF map enables a caller to enter the digits on the telephone keypad that correspond to the letters of the user's name or email alias. This attribute contains the values needed to create a DTMF map for the user's first name followed by the last name, for the user's last name followed by the first name, and for the user's email alias.

The following table shows the DTMF map values that would be stored in Active Directory on the **msExchUMDtmfMap** attribute for a UM-enabled user named Tony Smith with an alias of tsmith@contoso.com.

 **DTMF values stored for a UM-enabled user named Tony Smith**

|**Directory entry**|**User's name**|
|:-----|:-----|
| firstNameLastName:866976484|tonysmith|
| lastNameFirstName:764848669|smithtony|
| emailAddress:876484|tsmith|

Names and email aliases may contain other characters that aren't alphanumeric, such as commas, hyphens, underscores, or periods. Characters such as these won't be used in a DTMF map for a user. For example, if the email alias for Tony Smith is tony-smith@contoso.com, the DTMF map value would be 866976484, and the hyphen wouldn't be included. However, if a user's email alias contains a number or numbers, for example, tonysmith123@contoso.com, the numbers would be used in the DTMF map that's created. The DTMF map for tonysmith123 would be 866976484123.

A DTMF map must exist for a user for callers to be able to enter the user's name or email alias. However, not all users will have a DTMF map associated with their user account.

## DTMF maps for users who aren't enabled for Unified Messaging
<a name="dtmfmapsforusersnotenabled"> </a>

Users, including mailbox-enabled users, aren't enabled for Unified Messaging by default. The **msExchUMDtmfMap** attribute is populated with the values needed for DTMF maps for users who haven't been enabled for UM. By default, the following DTMF maps are created for all users when a mailbox is created for them:

1. emailAddress

2. firstNameLastName

3. lastNameFirstName

If a user doesn't have DTMF map values defined for their account, callers won't be able to contact the user when they press a telephone key from a UM auto attendant menu or perform a directory search. Also, UM-enabled users won't be able to send messages or transfer calls to users who don't have a DTMF map unless they can use Automatic Speech Recognition (ASR). To enable callers to transfer calls or contact users who aren't UM-enabled by using the telephone keypad, you need to create the necessary values for the DTMF map for those users. You can use the **Set-User** cmdlet with the  _-CreateDtmfMap_ parameter to create and update a single user's DTMF map or update a DTMF map for a user if the name of the user was changed after a DTMF map was created. Optionally, you can create an Exchange Management Shell script by using this cmdlet to update the DTMF map values for multiple users.

For more information about the **Set-User** cmdlet, see [Set-User](https://docs.microsoft.com/powershell/module/exchange/set-user).

## DTMF maps for users who are enabled for Unified Messaging
<a name="dtmfmapusersenabled"> </a>

 By default, a DTMF map is created for a user when they're enabled for Unified Messaging. This makes it possible for calls to be transferred to the UM-enabled user from external callers, from users who aren't enabled for UM, and from other UM-enabled users who use the telephone keypad to spell the user's name or email alias.

After the DTMF map values have been created for a UM-enabled user, callers can use the directory search feature. Callers use directory search when they use the telephone keypad in the following situations:

- To identify or search for a user when they call in to an Outlook Voice Access number.

- To locate or transfer calls to a UM-enabled user when they call in to a UM auto attendant.

For more information about how to enable a user for Unified Messaging, see [Enable a user for voice mail](enable-a-user-for-voice-mail-exchange-2013-help.md).

Sometimes a user's first name, last name, or email alias changes after the user is enabled for UM. The user's DTMF map values aren't updated automatically. If a caller enters the user's new name or email alias and the user's DTMF map hasn't been updated to reflect the change to the name or email alias, the caller won't be able to locate the user in the directory, send a message to the user, or transfer calls to the user. If you have to update a user's DTMF map after the user has been enabled for UM, you can use the **Set-User** cmdlet with the  _-CreateDtmfMap_ parameter. You can also create an Exchange Management Shell script using this cmdlet if you want to update the DTMF maps for multiple UM-enabled users.

> [!CAUTION]
> We recommend that you don't manually change the DTMF values for users by using a tool such as ADSI Edit because it might result in inconsistent configurations or other errors. We recommend that you use only the **Set-UMService** cmdlet or the **Set-User** cmdlet to create or update DTMF maps for users.

## For more information
<a name="fmi"> </a>

[Adsiedit Overview](https://go.microsoft.com/fwlink/p/?linkId=73175)
