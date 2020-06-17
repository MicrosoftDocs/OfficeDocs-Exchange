---
title: 'Update Exchange organization when Daylight Saving Time or time zone changes'
TOCTitle: Update your Exchange organization when Daylight Saving Time or the time zone changes
ms:assetid: 5b12615c-24cf-4f46-bf3c-2334dc734ef8
ms:mtpsurl: https://technet.microsoft.com/library/Hh530051(v=EXCHG.150)
ms:contentKeyID: 66452205
ms.reviewer:
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Update your Exchange organization when Daylight Saving Time or the time zone changes

_**Applies to:** Exchange Server 2013_

If the country or region where your organization or some of your users reside has changed their policy of recognizing Daylight Saving Time (DST), or changed the local time offset from Coordinated Universal Time (UTC), you need may need to update Microsoft Windows, Microsoft Exchange, Microsoft Outlook, or other programs to accommodate these changes.

For more information about DST changes around the world, including links, see the [Microsoft Daylight Saving Time Help and Support Center](https://support.microsoft.com/help/22803/daylight-saving-time). Also visit the support Web sites of your other software suppliers to see if they require any additional updates.

Even if your time zone hasn't changed, if you interact with other computers or users globally, your computer needs to be able to perform accurate date and time calculations for events elsewhere in the world.

Installing time zone updates as soon as possible minimizes the number of meetings or appointments that are scheduled during the transition from the old time and date to the new one.

## Step 1: Install the Windows DST update on all client and desktop computers

Because the Microsoft 365 and Office 365 authentication system is updated when DST or a time zone changes, all client computers need to be updated or they may experience connectivity issues.

Make sure all client and desktop computers have installed the Windows DST update. For more information, see [How to configure daylight saving time for Microsoft Windows operating systems](https://support.microsoft.com/help/914387/how-to-configure-daylight-saving-time-for-microsoft-windows-os).

## Step 2: Install the Windows DST update on all servers

1. Update all your on-premises servers with the Windows DST update.

2. If you're running Microsoft 365 or Office 365, update any servers that interact with the authentication system, such as DirSync or AD FS servers. These servers must be updated to ensure uptime.

**Note**: If you're updating server clusters, make sure you follow the usual process for updating clusters. You update the passive server first, fail over to the passive server (which becomes active), and then update the formerly active (now passive) server. For more information about how to update server clusters and high-availability server clusters, see [How to update Windows Server failover clusters](https://support.microsoft.com/help/174799).

## Step 3: Update Exchange and Outlook, where necessary, on client and desktop computers

1. If your users are using Outlook 2007 or older clients, determine which of the Exchange or Outlook time zone tools to run, using the table following this procedure.

2. Send a message to your users who need to update their computers, giving them a link to the appropriate tool.

The following table shows when users should [change time zone in Microsoft Outlook](https://support.office.com/article/5ab3e10e-5a6c-46af-ab48-156fedf70c04). Find which version your organization's servers are running and then determine which client programs your users are running.

<table summary="table">
<tbody>
<tr>
 <td> <p></p> </td>
 <td> <p> <strong>Client Version</strong> </p> </td>
 <td><p>&nbsp;</p></td>
 <td><p>&nbsp;</p></td>
 </tr>
<tr>
 <td> <p> <strong>Organization version</strong> </p> </td>
 <td> <p> <strong>Outlook 2003 or Outlook 2007</strong> </p> </td>
 <td> <p> <strong>Outlook 2010</strong> </p> </td>
 <td> <p> <strong>Outlook 2013</strong> </p> </td>
 <td><p>&nbsp;</p></td>
 </tr>
<tr>
 <td> <p> <strong>Exchange 2003 on premises</strong> </p> </td>
 <td> <p> <a href="https://support.office.com/article/add-remove-or-change-time-zones-5ab3e10e-5a6c-46af-ab48-156fedf70c04">Time Zone Data Update Tool for Microsoft Office Outlook</a> </p> </td>
 <td> <p>No action required</p> </td>
 <td> <p>No action required</p> </td>
 <td><p>&nbsp;</p></td>
 </tr>
<tr>
 <td> <p> <strong>Exchange 2007 on premises</strong> </p> </td>
 <td> <p> <a href="https://support.office.com/article/add-remove-or-change-time-zones-5ab3e10e-5a6c-46af-ab48-156fedf70c04">Time Zone Data Update Tool for Microsoft Office Outlook</a> </p> </td>
 <td> <p>No action required</p> </td>
 <td> <p>No action required</p> </td>
 <td><p>&nbsp;</p></td>
 </tr>
<tr>
 <td> <p> <strong>Exchange 2010 on premises</strong> </p> </td>
 <td> <p> <a href="https://support.office.com/article/add-remove-or-change-time-zones-5ab3e10e-5a6c-46af-ab48-156fedf70c04">Time Zone Data Update Tool for Microsoft Office Outlook</a> </p> </td>
 <td> <p>No action required</p> </td>
 <td> <p>No action required</p> </td>
 <td><p>&nbsp;</p></td>
 </tr>
<tr>
 <td> <p> <strong>Exchange 2013 on premises</strong> </p> </td>
 <td> <p> <a href="https://support.office.com/article/add-remove-or-change-time-zones-5ab3e10e-5a6c-46af-ab48-156fedf70c04">Time Zone Data Update Tool for Microsoft Office Outlook</a> </p> </td>
 <td> <p>No action required</p> </td>
 <td> <p>No action required</p> </td>
 <td><p>&nbsp;</p></td>
 </tr>
<tr>
 <td> <p> <strong>BPOS-S (Exchange 2007)</strong> </p> </td>
 <td> <p> <a href="https://support.office.com/article/add-remove-or-change-time-zones-5ab3e10e-5a6c-46af-ab48-156fedf70c04">Time Zone Data Update Tool for Microsoft Office Outlook</a> </p> </td>
 <td> <p>No action required</p> </td>
 <td> <p>No action required</p> </td>
 <td><p>&nbsp;</p></td>
 </tr>
<tr>
 <td> <p> <strong>BPOS-D (Exchange 2010)</strong> </p> </td>
 <td> <p> <a href="https://support.office.com/article/add-remove-or-change-time-zones-5ab3e10e-5a6c-46af-ab48-156fedf70c04">Time Zone Data Update Tool for Microsoft Office Outlook</a> </p> </td>
 <td> <p>No action required</p> </td>
 <td> <p>No action required</p> </td>
 <td><p>&nbsp;</p></td>
 </tr>
<tr>
 <td> <p> <strong>Microsoft 365 or Office 365 (Exchange 2010)</strong> </p> </td>
 <td> <p> <a href="https://support.office.com/article/add-remove-or-change-time-zones-5ab3e10e-5a6c-46af-ab48-156fedf70c04">Time Zone Data Update Tool for Microsoft Office Outlook</a> (not supported with Outlook 2003)</p> </td>
 <td> <p>No action required</p> </td>
 <td> <p>No action required</p> </td>
 <td><p>&nbsp;</p></td>
 </tr>
<tr>
 <td> <p> <strong>Microsoft 365 or Office 365 (Exchange 2013)</strong> </p> </td>
 <td> <p> <a href="https://support.office.com/article/add-remove-or-change-time-zones-5ab3e10e-5a6c-46af-ab48-156fedf70c04">Time Zone Data Update Tool for Microsoft Office Outlook</a> (not supported with Outlook 2003)</p> </td>
 <td> <p>No action required</p> </td>
 <td> <p>No action required</p> </td>
 <td><p>&nbsp;</p></td>
 </tr>
<tr>
 <td><p>&nbsp;</p></td>
 <td><p>&nbsp;</p></td>
 <td><p>&nbsp;</p></td>
 <td><p>&nbsp;</p></td>
 <td><p>&nbsp;</p></td>
 </tr>
 </tbody>
 </table>
