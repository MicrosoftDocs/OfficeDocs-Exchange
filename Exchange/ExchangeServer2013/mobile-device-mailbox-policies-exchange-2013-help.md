---
title: 'Mobile device mailbox policies: Exchange 2013 Help'
TOCTitle: Mobile device mailbox policies
ms:assetid: 9317b3bc-44a1-4e54-bc51-4f0b194b6a55
ms:mtpsurl: https://technet.microsoft.com/library/Bb123783(v=EXCHG.150)
ms:contentKeyID: 49318582
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Mobile device mailbox policies

_**Applies to:** Exchange Server 2013_

In Microsoft Exchange Server 2013, you can create mobile device mailbox policies to apply a common set of policies or security settings to a collection of users. After you deploy Exchange ActiveSync in your Exchange 2013 organization, you can create new mobile device mailbox policies or modify existing policies. When you install Exchange 2013, a default mobile device mailbox policy is created. All users are automatically assigned this default mobile device mailbox policy.

## Overview of mobile device mailbox policies

You can use mobile device mailbox policies to manage many different settings. These include the following:

  - Require a password.

  - Specify the minimum password length.

  - Require a number or special character in the password.

  - Designate how long a device can be inactive before requiring the user to re-enter a password.

  - Wipe a device after a specific number of failed password attempts.

For more information about all the settings you can configure, see Mobile device policy settings.

## Managing Exchange ActiveSync mailbox policies

Mobile device mailbox policies can be created in the Exchange admin center (EAC) or the Exchange Management Shell. If you create a policy in the EAC, you can configure only a subset of the available settings. You can configure the rest of the settings using the Shell.

## Windows Phone 7 synchronization

If you have Windows Phone 7 mobile phones in your organization, these phones will experience synchronization problems if certain Exchange ActiveSync mailbox policy properties are configured. To allow Windows Phone 7 mobile phones to synchronize with an Exchange mailbox, either set the **AllowNonProvisionableDevices** property to True or only configure the following Exchange ActiveSync mailbox policy properties:

  - AllowSimplePassword

  - BlockInternetSharing

  - BlockRemoteDesktop

  - DisableDesktopSync

  - DisableIrDA

  - DisableRemovableStorage

  - DeviceWipeThreshold

  - MinPasswordLength

  - IdleTimeoutFrequencyValue

  - PasswordExpiration

  - PasswordHistory

  - PasswordRequired

## Mobile device mailbox policy settings

The following table summarizes the settings you can specify using mobile device mailbox policies.

### Mobile device mailbox policy settings

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Setting</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Allow Bluetooth</p></td>
<td><p>This setting specifies whether a mobile device allows Bluetooth connections. The available options are Disable, HandsFree Only, and Allow. The default value is Allow. The Exchange Enterprise Client Access License is required to change the values of this setting.</p></td>
</tr>
<tr class="even">
<td><p>Allow Browser</p></td>
<td><p>This setting specifies whether Pocket Internet Explorer is allowed on the mobile device. This setting doesn't affect third-party browsers installed on the mobile device. The default value is <code>$true</code>. The Exchange Enterprise Client Access License is required to change the values of this setting.</p></td>
</tr>
<tr class="odd">
<td><p>Allow Camera</p></td>
<td><p>This setting specifies whether the mobile device camera can be used. The default value is <code>$true</code>. The Exchange Enterprise Client Access License is required to change the values of this setting.</p></td>
</tr>
<tr class="even">
<td><p>Allow Consumer EMail</p></td>
<td><p>This setting specifies whether the mobile device user can configure a personal email account (either POP3 or IMAP4) on the mobile device. The default value is <code>$true</code>. This setting doesn't control access to email accounts that are using third-party mobile device email programs. The Exchange Enterprise Client Access License is required to change the values of this setting.</p></td>
</tr>
<tr class="odd">
<td><p>Allow Desktop Sync</p></td>
<td><p>This setting specifies whether the mobile device can synchronize with a computer through a cable, Bluetooth, or IrDA connection. The default value is <code>$true</code>. The Exchange Enterprise Client Access License is required to change the values of this setting.</p></td>
</tr>
<tr class="even">
<td><p>Allow External Device Management</p></td>
<td><p>This setting specifies whether an external device management program is allowed to manage the mobile device.</p></td>
</tr>
<tr class="odd">
<td><p>Allow HTML Email</p></td>
<td><p>This setting specifies whether email synchronized to the mobile device can be in HTML format. If this setting is set to <code>$false</code>, all email is converted to plain text.</p></td>
</tr>
<tr class="even">
<td><p>Allow Internet Sharing</p></td>
<td><p>This setting specifies whether the mobile device can be used as a modem for a desktop or a portable computer. The default value is <code>$true</code>. The Exchange Enterprise Client Access License is required to change the values of this setting.</p></td>
</tr>
<tr class="odd">
<td><p>AllowIrDA</p></td>
<td><p>This setting specifies whether infrared connections are allowed to and from the mobile device. The Exchange Enterprise Client Access License is required to change the values of this setting.</p></td>
</tr>
<tr class="even">
<td><p>Allow Mobile OTA Update</p></td>
<td><p>This setting specifies whether the mobile device mailbox policy settings can be sent to the mobile device over a cellular data connection. The default value is <code>$true</code>.</p></td>
</tr>
<tr class="odd">
<td><p>Allow non-provisionable devices</p></td>
<td><p>This setting specifies whether mobile devices that may not support application of all policy settings are allowed to connect to Exchange 2013 by using Exchange ActiveSync. Allowing non-provisionable mobile devices has security implications. For example, some non-provisionable devices may not be able to implement an organization's password requirements.</p></td>
</tr>
<tr class="even">
<td><p>Allow POPIMAPEmail</p></td>
<td><p>This setting specifies whether the user can configure a POP3 or an IMAP4 email account on the mobile device. The default value is <code>$true</code>. This setting doesn't control access by third-party email programs.</p></td>
</tr>
<tr class="odd">
<td><p>Allow Remote Desktop</p></td>
<td><p>This setting specifies whether the mobile device can initiate a remote desktop connection. The default value is <code>$true</code>. The Exchange Enterprise Client Access License is required to change the values of this setting.</p></td>
</tr>
<tr class="even">
<td><p>Allow simple password</p></td>
<td><p>This setting enables or disables the ability to use a simple password such as 1111 or 1234. The default value is <code>$true</code>.</p></td>
</tr>
<tr class="odd">
<td><p>Allow S/MIME encryption algorithm negotiation</p></td>
<td><p>This setting specifies whether the messaging application on the mobile device can negotiate the encryption algorithm if a recipient's certificate doesn't support the specified encryption algorithm.</p></td>
</tr>
<tr class="even">
<td><p>Allow S/MIME software certificates</p></td>
<td><p>This setting specifies whether S/MIME software certificates are allowed on the mobile device.</p></td>
</tr>
<tr class="odd">
<td><p>Allow storage card</p></td>
<td><p>This setting specifies whether the mobile device can access information that's stored on a storage card. The Exchange Enterprise Client Access License is required to change the values of this setting.</p></td>
</tr>
<tr class="even">
<td><p>Allow text messaging</p></td>
<td><p>This setting specifies whether text messaging is allowed from the mobile device. The default value is <code>$true</code>. The Exchange Enterprise Client Access License is required to change the values of this setting.</p></td>
</tr>
<tr class="odd">
<td><p>Allow unsigned applications</p></td>
<td><p>This setting specifies whether unsigned applications can be installed on the mobile device. The default value is <code>$true</code>. The Exchange Enterprise Client Access License is required to change the values of this setting.</p></td>
</tr>
<tr class="even">
<td><p>Allow unsigned installation packages</p></td>
<td><p>This setting specifies whether an unsigned installation package can be run on the mobile device. The default value is <code>$true</code>. The Exchange Enterprise Client Access License is required to change the values of this setting.</p></td>
</tr>
<tr class="odd">
<td><p>Allow Wi-Fi</p></td>
<td><p>This setting specifies whether wireless Internet access is allowed on the mobile device. The default value is <code>$true</code>. The Exchange Enterprise Client Access License is required to change the values of this setting.</p></td>
</tr>
<tr class="even">
<td><p>Alphanumeric password required</p></td>
<td><p>This setting requires that a password contains numeric and non-numeric characters. The default value is <code>$true</code>.</p></td>
</tr>
<tr class="odd">
<td><p>Approved Application List</p></td>
<td><p>This setting stores a list of approved applications that can be run on the mobile device. The Exchange Enterprise Client Access License is required to change the values of this setting.</p></td>
</tr>
<tr class="even">
<td><p>Attachments enabled</p></td>
<td><p>This setting enables attachments to be downloaded to the mobile device. The default value is <code>$true</code>.</p></td>
</tr>
<tr class="odd">
<td><p>Device encryption enabled</p></td>
<td><p>This setting enables encryption on the mobile device. Not all mobile devices can enforce encryption. For more information, see the device and mobile operating system documentation.</p></td>
</tr>
<tr class="even">
<td><p>Device policy refresh interval</p></td>
<td><p>This setting specifies how often the mobile device mailbox policy is sent from the server to the mobile device.</p></td>
</tr>
<tr class="odd">
<td><p>IRM enabled</p></td>
<td><p>This setting specifies whether Information Rights Management (IRM) is enabled on the mobile device.</p></td>
</tr>
<tr class="even">
<td><p>Max attachment size</p></td>
<td><p>This setting controls the maximum size of attachments that can be downloaded to the mobile device. The default value is Unlimited.</p></td>
</tr>
<tr class="odd">
<td><p>Max calendar age filter</p></td>
<td><p>This setting specifies the maximum range of calendar days that can be synchronized to the mobile device. The following values are accepted:</p>
<ol>
<li><p>All</p></li>
<li><p>OneDay</p></li>
<li><p>ThreeDays</p></li>
<li><p>OneWeek</p></li>
<li><p>TwoWeeks</p></li>
<li><p>OneMonth</p></li>
</ol></td>
</tr>
<tr class="even">
<td><p>Max email age filter</p></td>
<td><p>This setting specifies the maximum number of days of email items to synchronize to the mobile device. The following values are accepted:</p>
<ol>
<li><p>All</p></li>
<li><p>OneDay</p></li>
<li><p>ThreeDays</p></li>
<li><p>OneWeek</p></li>
<li><p>TwoWeeks</p></li>
<li><p>OneMonth</p></li>
</ol></td>
</tr>
<tr class="odd">
<td><p>Max email body truncation size</p></td>
<td><p>This setting specifies the maximum size at which email messages are truncated when synchronized to the mobile device. The value is in kilobytes (KB).</p></td>
</tr>
<tr class="even">
<td><p>Max email HTML body truncation size</p></td>
<td><p>This setting specifies the maximum size at which HTML email messages are truncated when synchronized to the mobile device. The value is in kilobytes (KB).</p></td>
</tr>
<tr class="odd">
<td><p>Max inactivity time lock</p></td>
<td><p>This value specifies the length of time that the mobile device can be inactive before a password is required to reactivate it. You can enter any interval between 30 seconds and 1 hour. The default value is 15 minutes.</p></td>
</tr>
<tr class="even">
<td><p>Max password failed attempts</p></td>
<td><p>This setting specifies the number of attempts a user can make to enter the correct password for the mobile device. You can enter any number from 4 through 16. The default value is 8.</p></td>
</tr>
<tr class="odd">
<td><p>Min password complex characters</p></td>
<td><p>This setting specifies the number of character sets that are required in the password of the mobile device. The character sets are:</p>
<ul>
<li><p>Lower case letters.</p></li>
<li><p>Upper case letters.</p></li>
<li><p>Digits 0 through 9.</p></li>
<li><p>Special characters (for example, exclamation marks).</p></li>
</ul>
<p>You can enter any number from 1 through 4. The default value is 1.</p>
<p>For Windows Phone 8 devices, this setting specifies the number of character sets that are required in the password. For example, the value 3 requires at least one character from any three of the character sets.</p>
<p>For Windows Phone 10 devices, this setting specifies the following password complexity requirements:</p>
<ol>
<li><p>Digits only.</p></li>
<li><p>Digits and lower case letters.</p></li>
<li><p>Digits, lower case letters, and upper case letters.</p></li>
<li><p>Digits, lower case letters, upper case letters, and special characters.</p></li>
</ol></td>
</tr>
<tr class="even">
<td><p>Min password length</p></td>
<td><p>This setting specifies the minimum number of characters in the mobile device password. You can enter any number from 1 through 16. The default value is 4.</p></td>
</tr>
<tr class="odd">
<td><p>Password enabled</p></td>
<td><p>This setting enables the mobile device password.</p></td>
</tr>
<tr class="even">
<td><p>Password expiration</p></td>
<td><p>This setting enables the administrator to configure a length of time after which a mobile device password must be changed.</p></td>
</tr>
<tr class="odd">
<td><p>Password history</p></td>
<td><p>This setting specifies the number of past passwords that can be stored in a user's mailbox. A user can't reuse a stored password.</p></td>
</tr>
<tr class="even">
<td><p>Password recovery enabled</p></td>
<td><p>When this setting is enabled, the mobile device generates a recovery password that's sent to the server. If the user forgets their mobile device password, the recovery password can be used to unlock the mobile device and enable the user to create a new mobile device password.</p></td>
</tr>
<tr class="odd">
<td><p>Require device encryption</p></td>
<td><p>This setting specifies whether device encryption is required. If set to <code>$true</code>, the mobile device must be able to support and implement encryption to synchronize with the server.</p></td>
</tr>
<tr class="even">
<td><p>Require encrypted S/MIME messages</p></td>
<td><p>This setting specifies whether S/MIME messages must be encrypted. The default value is <code>$false</code>.</p></td>
</tr>
<tr class="odd">
<td><p>Require encryption S/MIME algorithm</p></td>
<td><p>This setting specifies what required algorithm must be used when encrypting S/MIME messages.</p></td>
</tr>
<tr class="even">
<td><p>Require manual synchronization while roaming</p></td>
<td><p>This setting specifies whether the mobile device must synchronize manually while roaming. Allowing automatic synchronization while roaming will frequently lead to larger-than-expected data costs for the mobile device data plan.</p></td>
</tr>
<tr class="odd">
<td><p>Require signed S/MIME algorithm</p></td>
<td><p>This setting specifies what required algorithm must be used when signing a message.</p></td>
</tr>
<tr class="even">
<td><p>Require signed S/MIME messages</p></td>
<td><p>This setting specifies whether the mobile device must send signed S/MIME messages.</p></td>
</tr>
<tr class="odd">
<td><p>Require storage card encryption</p></td>
<td><p>This setting specifies whether the storage card must be encrypted. Not all mobile device operating systems support storage card encryption. For more information, see your mobile device and mobile operating system documentation.</p></td>
</tr>
<tr class="even">
<td><p>Unapproved InROM application list</p></td>
<td><p>This setting specifies a list of applications that cannot be run in ROM. The Exchange Enterprise Client Access License is required to change the values of this setting.</p></td>
</tr>
</tbody>
</table>
