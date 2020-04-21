---
localization_priority: Normal
description: 'Summary: This article describes how users of Outlook for iOS and Android can use third-party add-ins for online meetings'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
title: Using third-party add-ins for online meetings in Outlook for iOS and Android
ms.custom: remotework 
ms.reviewer: smithre4
audience: ITPro
ms.service: exchange-online
f1.keywords:
- NOCSH
manager: serdars

---

# Using third-party add-ins for online meetings in Outlook for iOS and Android
 
Setting up an online meeting is a core experience for Outlook users. To meet the needs of an increasing number of remote workers and students, Outlook for iOS and Android has enabled add-ins to provide online meetings from third-party providers such as Zoom, BlueJeans, and Webex (among others). End-users in your organization will be able to use these add-ins to set up online meetings on third-party platforms.

> [!NOTE]
> Currently this functionality is only available in Outlook for Android. It will be available in Outlook for iOS soon.

## How to enable third-party online meeting integration with Outlook for iOS and Android

Third-party online meeting integration is handled by add-ins that have enabled this functionality specifically for Outlook for iOS and Android. To set up this third-party meeting creation experience, an Exchange Online administrator must install the supported add-in(s) to the mailbox of an end user. Once installed, the third-party meeting creation button will appear on users' screens instead of a Microsoft Teams or Skype meeting button.

> [!NOTE]
> Add-ins installed by your end users will **not** override the default Teams or Skype functionality.

The add-ins can be deployed using the following admin portals:

- If all users are Microsoft 365 users, then use the [centralized deployment portal](https://docs.microsoft.com/office/dev/add-ins/publish/centralized-deployment). Centralized deployment provides the capability to install add-ins more granularly, such as to sub-groups within a given organization.
    
- If a tenant has users' mailboxes in Exchange Server on premises, then use the ECP/EAC portal. More information is available [here](https://docs.microsoft.com/Exchange/install-or-remove-outlook-add-ins-2013-help).


## Creating an online meeting with a third-party add-in

The third-party online meeting provider will appear on the event creation screen in Outlook for iOS and Android, as displayed below. The third-party add-in replaces the Teams or the Skype buttons, but the button users do see will act in a similar way. After tapping the toggle button, the online meeting URL and text is retrieved from the third-party service and is inserted into the meeting body.

Users cannot save the meeting until the online meeting details have been retrieved.

:::image type="content" source="../../media/android-meeting-creation.png" alt-text="creating a meeting with a third-party add-in on Outlook for Android":::


## Meeting providers displayed in the New Event screen

On a user's New Event screen, only a single meeting provider will be shown. If there are multiple options, the logic to select which provider is displayed is as follows:

  - Priority 1: Any custom online meeting add-ins that are installed (this is a developer scenario also known as "side loading").

  - Priority 2: An online meeting add-in that was installed by an administrator.

  - Default selection: If there are no admin-installed online Web conferencing add-ins, the default option of Teams and Skype will be shown, as described [in this article](https://docs.microsoft.com/microsoftteams/setting-your-coexistence-and-upgrade-settings).

> [!NOTE]
> Installing multiple add-in providers for online meetings on a user's device isn't supported and may result in unexpected behavior.


## Developing add-ins for remote meetings in Outlook for iOS and Android

Add-in developers need to add the MobileOnlineMeetingCommandSurface extension point in their add-in manifest. 

Information for add-in developers is available in [Create an Outlook mobile add-in for an online-meeting provider](https://docs.microsoft.com/office/dev/add-ins/outlook/online-meeting).

Capabilities exposed to online meeting add-ins include:

  - UI-less command. Online meeting add-ins can only run in a UI-less mode, which means the add-ins don't have the capability to launch a task pane.

  - Display dialogue. Login flow can be handled using full-screen dialog.

  - The specific APIs that are exposed are [listed here](https://docs.microsoft.com/office/dev/add-ins/outlook/online-meeting\#available-apis).


## How users join meetings

Support for third-party remote meeting add-ins includes making it easy for users to join meetings. A **Join** button gets added to users' calendar events in Outlook for iOS and Android. Clicking **Join** will launch the online meeting app, if the user has it installed. If the app is not installed, the browser will launch and guide the user through the process to join the meeting. 

Note that recipients of meeting invitations don't need to have the add-in for the corresponding third-party meeting provider installed on their devices in order to join the meeting.

:::image type="content" source="../../media/android-join.png" alt-text="Joining a meeting with a third-party meeting provider in Outlook for Android":::