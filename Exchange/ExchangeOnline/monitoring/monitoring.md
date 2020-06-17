---
localization_priority: Normal
description: Exchange Online offers many different reports that can help you determine the overall status and health of your organization. There are also tools to help you troubleshoot specific events (such as a message not arriving to its intended recipients), and auditing reports to aid with compliance requirements. The following table describes the reports and troubleshooting tools that are available to Exchange Online administrators.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 87bdeeae-bd80-4a3b-95c5-62fbaf97c2e8
ms.reviewer: 
f1.keywords:
- NOCSH
title: Monitoring, reporting, and message tracing in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Monitoring, reporting, and message tracing in Exchange Online

Exchange Online offers many different reports that can help you determine the overall status and health of your organization. There are also tools to help you troubleshoot specific events (such as a message not arriving to its intended recipients), and auditing reports to aid with compliance requirements. The following table describes the reports and troubleshooting tools that are available to Exchange Online administrators.

> [!NOTE]
> For a mapping of reports from the old Microsoft 365 admin center, see [Where did my Office 365 report go?](https://go.microsoft.com/fwlink/p/?linkid=817242)

|**Feature**|**Description**|**Where you can find it**|**For more information**|
|:-----|:-----|:-----|:-----|
|**Usage reports in the Microsoft 365 admin center**|**Microsoft 365 groups activity**: View information about the number of Microsoft 365 groups that are created and used. <br/> **Email activity**: View information about the number of messages sent, received and read in your whole organization, and by specific users. <br/> **Email app usage**: View information about the email apps that are connecting to Exchange Online. This include the total number of connections for each app, and the versions of Outlook that are connecting. <br/> **Mailbox usage**: View information about storage used, quota consumption, item count, and last activity (send or read activity) for mailboxes.|In the Microsoft 365 admin center at <https://portal.office.com/adminportal/home>, click **Reports** \> **Usage**. At the top of the dashboard, click Select a report. In the  in the drop-down list that appears, make one of these selections: <br/> Office 365 section: Microsoft 365 groups activity <br/>Exchange section:  Email activity <br/>Email app usageMailbox usage|[Microsoft 365 Reports in the admin center - Microsoft 365 groups](https://docs.microsoft.com/microsoft-365/admin/activity-reports/office-365-groups) <br/> [Microsoft 365 Reports in the Admin Center - Email activity](https://docs.microsoft.com/microsoft-365/admin/activity-reports/email-activity) <br/> [Microsoft 365 Reports in the Admin Center - Email apps usage](https://docs.microsoft.com/microsoft-365/admin/activity-reports/email-apps-usage) <br/> [Microsoft 365 Reports in the Admin Center - Mailbox usage](https://docs.microsoft.com/microsoft-365/admin/activity-reports/mailbox-usage)|
|**Security & compliance reports in the Microsoft 365 admin center**| These enhanced reports provide an interactive reporting experience for Exchange Online admins, which includes summary information, and the ability to drill down for more details. <br/> **Data loss prevention (DLP)**: View information about DLP policies and rules that affect messages containing sensitive data as they enter and leave your organization. <br/> **Note**: DLP is only available in certain Exchange Online subscription plans. For information, see the **Data Loss Prevention** entries in the [Exchange Online Service Description](https://docs.microsoft.com/office365/servicedescriptions/exchange-online-service-description/exchange-online-service-description). <br/> **Advanced Threat Protection (ATP)**: View information about safe links and safe attachments that are part of ATP. <br/> **Note**: ATP is available in Office 365 Enterprise E5, but you can also purchase ATP as an add-on to other subscription plans. For more information, see [Office 365 Advanced Threat Protection Service Description](https://docs.microsoft.com/office365/servicedescriptions/office-365-advanced-threat-protection-service-description). <br/> **Exchange Online Protection (EOP)**: View information about malware detections, spoofed mail, spam detections, and mail flow to and from your organization.|In the Microsoft 365 Security Center at <https://protection.office.com>, click **Reports** \> **Dashboard**. Select one of the reports that are available on the page: <br/> DLP reports: DLP policy matches and DLP false positives and overrides. <br/> ATP reports: ATP file types, ATP message disposition, and Threat protection status. <br/> EOP reports: Malware detections, Top malware, Top senders and recipients, Spoof mail, Spam detections, and Sent and received mail.|[View the reports for data loss prevention](https://docs.microsoft.com/microsoft-365/compliance/view-the-dlp-reports) <br/> [View reports for Office 365 Advanced Threat Protection](https://docs.microsoft.com/microsoft-365/security/office-365-security/view-reports-for-atp)|
|**Custom reports using Microsoft Graph**|Programmatically create the reports that are available in the Microsoft 365 admin center by using Microsoft Graph|n/a|The subtopics of [Working with Office 365 usage reports in Microsoft Graph](https://docs.microsoft.com/graph/api/resources/report)|
|**Custom reports using reporting web services**|Programmatically create reports from the available Exchange Online PowerShell reporting cmdlets by using REST/ODATA2 query filtering. <br/> **Note**: Many of the original Exchange Online PowerShell reporting cmdlets have been deprecated and replaced by similar reports in Microsoft Graph. For more information, see [Reporting cmdlets in Exchange Online](https://docs.microsoft.com/powershell/exchange/exchange-online-powershell).|<https://reports.office365.com/ecp/reportingwebservice/reporting.svc>|[Office 365 Reporting Web Services](https://go.microsoft.com/fwlink/p/?LinkId=279926)|
|**Message trace**|Follows email messages as they travel through your Exchange Online organization. You can determine if an email message was received, rejected, deferred, or delivered by the service. It also shows what actions were taken on the message before it reached its final status. <br/> You can use this information to efficiently answer your user's questions, troubleshoot mail flow issues, validate policy changes, and alleviates the need to contact technical support for assistance.|For more information, see [Message trace in the Security & Compliance Center](https://docs.microsoft.com/microsoft-365/security/office-365-security/message-trace-scc).|
|**Audit logging**|Tracks specific changes made by admins to your Exchange Online organization. These reports help you meet regulatory, compliance, and litigation requirements.|For more information, see [View the administrator audit log](../security-and-compliance/exchange-auditing-reports/view-administrator-audit-log.md).|[Exchange auditing reports](../security-and-compliance/exchange-auditing-reports/exchange-auditing-reports.md)|

## Reporting and message trace data availability and latency

The following table describes when Exchange Online reporting and message trace data is available and for how long.

|**Report type**|**Data available for (look back period)**|**Latency**|
|:-----|:-----|:-----|
|Mailbox summary reports|60 days|Message data aggregation is mostly complete within 24-48 hours. Some minor incremental aggregated changes may occur for up to 5 days.|
|Mail protection summary reports|90 days|Message data aggregation is mostly complete within 24-48 hours. Some minor incremental aggregated changes may occur for up to 5 days.|
|Mail protection detail reports|90 days|For detail data that's less than 7 days old, data should appear within 24 hours but may not be complete until 48 hours. Some minor incremental changes may occur for up to 5 days. <br/> To view detail reports for messages that are greater than 7 days old, results may take up to a few hours.|
|Message trace data|90 days|When you run a message trace for messages that are less than 7 days old, the messages should appear within 5-30 minutes. <br/> When you run a message trace for messages that are greater than 7 days old, results may take up to a few hours.|

> [!NOTE]
> Data availability and latency is the same whether requested via the Microsoft 365 admin center or remote PowerShell.
