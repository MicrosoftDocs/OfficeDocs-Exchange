---
localization_priority: Normal
description: 'Summary: Exchange ActiveSync is a client protocol that lets you synchronize a mobile device with your Exchange mailbox. Exchange ActiveSync is enabled by default when you install Exchange Server 2016 or Exchange Server 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 5fafaff3-eb37-4fdb-95f0-e56c45ea5884
ms.reviewer: 
title: Exchange ActiveSync
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Exchange ActiveSync

Exchange ActiveSync is an Exchange synchronization protocol that's optimized to work together with high-latency and low-bandwidth networks. The protocol, based on HTTP and XML, lets mobile phones access an organization's information on a server that's running Microsoft Exchange.

## Overview of Exchange ActiveSync
<a name="overview"> </a>

 Exchange ActiveSync lets mobile phone users access their email, calendar, contacts, and tasks, and lets them continue to access this information when they're working offline.

Standard encryption services add security to mobile communication with the server. You can configure Exchange ActiveSync to use Secure Sockets Layer (SSL) encryption for communications between the Exchange server and the mobile device.

## Features in Exchange ActiveSync
<a name="features"> </a>

Exchange ActiveSync provides the following:

- Support for HTML messages

- Support for follow-up flags

- Conversation grouping of email messages

- Ability to synchronize or not synchronize an entire conversation

- Synchronization of Short Message Service (SMS) messages with a user's Exchange mailbox

- Support for viewing message reply status

- Support for fast message retrieval

- Meeting attendee information

- Enhanced Exchange Search

- PIN reset

- Enhanced device security through password policies

- Autodiscover for over-the-air provisioning

- Support for setting automatic replies when users are away, on vacation, or out of the office

- Support for task synchronization

- Direct Push

- Support for availability information for contacts

## Managing Exchange ActiveSync
<a name="managing"> </a>

By default, Exchange ActiveSync is enabled. All users who have an Exchange mailbox can synchronize their mobile device with the Microsoft Exchange server.

You can perform the following Exchange ActiveSync tasks:

- Enable and disable Exchange ActiveSync for users

- Set policies such as minimum password length, device locking, and maximum failed password attempts

- Initiate a remote wipe to clear all data from a lost or stolen mobile phone

- Run a variety of reports for viewing or exporting into a variety of formats

- Control which types of mobile devices can synchronize with your organization through device access rules

### Managing mobile device access in Exchange ActiveSync

You can control which mobile devices can synchronize with Exchange Server. You do this by monitoring new mobile devices as they connect to your organization or by setting up rules that determine which types of mobile devices are allowed to connect. Regardless of the method you choose to specify which mobile devices can synchronize, you can approve or deny access for any specific mobile device for a specific user at any time.

### Device security features in Exchange ActiveSync

In addition to the ability to configure security options for communications between the Exchange server and your mobile devices, Exchange ActiveSync offers the following features to enhance the security of mobile devices:

- **Remote wipe**: If a mobile device is lost, stolen, or otherwise compromised, you can issue a remote wipe command from the Exchange Server computer or from any Web browser by using Outlook Web App. This command erases all data from the mobile device.

- **Device password policies**: Exchange ActiveSync lets you configure several options for device passwords. The device password options include the following:

  - **Minimum password length (characters)**: This option specifies the length of the password for the mobile device. The default length is 4 characters, but as many as 18 can be included.

  - **Minimum number of character sets**: Use this text box to specify the complexity of the alphanumeric password and force users to use a number of different sets of characters from among the following: lowercase letters, uppercase letters, symbols, and numbers.

  - **Require alphanumeric password**: This option determines password strength. You can enforce the usage of a character or symbol in the password in addition to numbers.

  - **Inactivity time (seconds)**: This option determines how long the mobile device must be inactive before the user is prompted for a password to unlock the mobile device.

  - **Enforce password history**: Select this check box to force the mobile phone to prevent the user from reusing their previous passwords. The number that you set determines the number of past passwords that the user won't be allowed to reuse.

  - **Enable password recovery**: Select this check box to enable password recovery for the mobile device. Administrators can use the **Get-ActiveSyncDeviceStatistics** cmdlet to look up the user's recovery password.

  - **Wipe device after failed (attempts)**: This option lets you specify whether you want the phone's memory to be wiped after multiple failed password attempts.

- **Device encryption policies**: There are a number of mobile device encryption policies that you can enforce for a group of users. These policies include the following:

  - **Require encryption on device**: Select this check box to require encryption on the mobile device. This increases security by encrypting all information on the mobile device.

  - **Require encryption on storage cards**: Select this check box to require encryption on the mobile device's removable storage card. This increases security by encrypting all information on the storage cards for the mobile device.
