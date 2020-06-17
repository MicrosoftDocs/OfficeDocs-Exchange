---
title: 'Information Rights Management in Exchange ActiveSync: Exchange 2013 Help'
TOCTitle: Information Rights Management in Exchange ActiveSync
ms:assetid: ebf04460-4d61-4b00-86b9-85ec1dbbd6a1
ms:mtpsurl: https://technet.microsoft.com/library/Ff657743(v=EXCHG.150)
ms:contentKeyID: 49319938
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Information Rights Management in Exchange ActiveSync

_**Applies to:** Exchange Server 2013_

Information workers often use e-mail to exchange sensitive information. To help secure this information, organizations can use Information Rights Management (IRM) to apply persistent protection to messaging content. Because mobile devices are increasingly being used to access e-mail, it's important that your mobile device users be able to create and consume IRM-protected content.

## Mobile IRM protection in Exchange 2013

In Exchange 2013, IRM in Microsoft Exchange ActiveSync allows your users to access rich IRM functionality on any supported Exchange ActiveSync device without having to configure AD RMS permissions or connect the device to a computer and activate it for IRM. Also, the mobile device doesn't need to be running Windows. Exchange ActiveSync is licensed by Microsoft to mobile device manufacturers, original equipment manufacturers (OEMs), and others. For a list of current Exchange ActiveSync licensees, expand the "Exchange ActiveSync Protocol" section on the [Microsoft Technology Licensing](https://go.microsoft.com/fwlink/p/?linkid=198562) page.

Using IRM in Exchange ActiveSync, mobile device users can:

- Create IRM-protected messages.

- Read IRM-protected messages.

- Reply to and forward IRM-protected messages.

## Requirements

The following requirements apply:

- The Client Access servers in your organization must be running Exchange 2010 SP1 or later.

- An AD RMS server must be deployed in your organization.

- IRM must be enabled for internal messages. This is a prerequisite for all IRM features in Exchange 2010. For details, see [Enable or Disable IRM for Internal Messages](enable-or-disable-irm-for-internal-messages-exchange-2013-help.md).

- IRM must be enabled in the Exchange ActiveSync mailbox policy. You can enable or disable IRM for different sets of users using different Exchange ActiveSync mailbox policies.

- Devices that support Exchange ActiveSync protocol version 14.1, including Windows phones, can support IRM in Exchange ActiveSync. The device's mobile e-mail application must support the RightsManagementInformation tag defined in Exchange ActiveSync version 14.1.

## Security

When you enable IRM in Exchange ActiveSync, the Client Access server decrypts IRM-protected messages before providing the messages for access by the supported mobile device. Upon synchronization, IRM-protected messages reside on the mobile device in an unencrypted format. IRM protection is enforced by the IRM-capable e-mail client application on the mobile device.

IRM in Exchange ActiveSync doesn't decrypt IRM-protected attachments on the Client Access server. Access to IRM-protected files is enforced by the application used to create or view the file. For example, on a Windows phone, IRM protection for Microsoft Office files is enforced by [Microsoft Office Mobile](https://www.microsoft.com/microsoft-365/mobile). To access IRM-protected Office files, users must connect the device to a computer and activate Office Mobile with the RMS server.

When enabling IRM in Exchange ActiveSync, we recommend using the Exchange ActiveSync policy settings shown in the following table to help secure mobile devices.

### Exchange ActiveSync policy settings

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Setting</th>
<th>Configure using the New Exchange ActiveSync Mailbox Policy wizard</th>
<th>Configure using the New-ActiveSyncMailboxPolicy cmdlet</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Require that the user enter a password to access information on their mobile device.</p></td>
<td><p>Select the <strong>Require password</strong> check box.</p></td>
<td><p>Set the <em>DevicePasswordEnabled</em> parameter to <code>$true</code>.</p></td>
</tr>
<tr class="even">
<td><p>Enable encryption for the mobile device.</p></td>
<td><p>Select the <strong>Require password</strong> check box, and then select the <strong>Require encryption on device</strong> check box.</p></td>
<td><p>Set the <em>RequireDeviceEncryption</em> parameter to <code>$true</code>.</p>

> [!IMPORTANT]
> When you set the <EM>RequireDeviceEncryption</EM> parameter to <CODE>$true</CODE>, mobile devices that don't support device encryption will be unable to connect.

</td>
</tr>
<tr class="odd">
<td><p>Don't allow non-provisionable mobile devices to synchronize with the Exchange server.</p></td>
<td><p>Clear the <strong>Allow non-provisionable devices</strong> check box.</p></td>
<td><p>Set the <em>AllowNonProvisionableDevices</em> parameter to <code>$false</code>.</p></td>
</tr>
</tbody>
</table>

To learn more, see [Mobile device mailbox policies](mobile-device-mailbox-policies-exchange-2013-help.md).

## Enabling IRM in Exchange ActiveSync

To enable IRM in Exchange ActiveSync, perform the following tasks:

1. Add the Federation mailbox (a system mailbox created by Exchange 2013 and Exchange 2010 Setup) to the super users group in AD RMS. This allows Exchange 2013 and Exchange 2010 servers to access IRM-protected messages. For details, see [Add the Federation Mailbox to the AD RMS Super Users Group](add-the-federation-mailbox-to-the-ad-rms-super-users-group-exchange-2013-help.md).

2. Use the [Set-IRMConfiguration](https://docs.microsoft.com/powershell/module/exchange/Set-IRMConfiguration) cmdlet in the Exchange Management Shell to enable IRM on the Client Access server. This enables IRM in Exchange ActiveSync and IRM in Microsoft Office Outlook Web App for your organization. For details, see [Enable or Disable Information Rights Management on Client Access Servers](enable-or-disable-information-rights-management-on-client-access-servers-exchange-2013-help.md).
