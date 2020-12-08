---
localization_priority: Normal
description: Exchange Online offers many different reports that can help you determine the overall status and health of your organization. There are also tools to help you troubleshoot specific events (such as a message not arriving to its intended recipients), and auditing reports to aid with compliance requirements. The following table describes the reports and troubleshooting tools that are available to Exchange Online administrators.
ms.topic: article
author: msdmaguire
ms.author: dmaguire
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
> For a mapping of reports from the old Microsoft 365 admin center, see [Where did my report go?](https://support.microsoft.com/office/4f7ce026-8be0-4800-849c-28071df0b85f)

****

|Feature|Reports|Location|
|---|---|---|
|**Reports in the Microsoft 365 admin center**|[Email activity](https://docs.microsoft.com/microsoft-365/admin/activity-reports/email-activity) <p> [Email app usage](https://docs.microsoft.com/microsoft-365/admin/activity-reports/email-apps-usage) <p> [Mailbox usage](https://docs.microsoft.com/microsoft-365/admin/activity-reports/mailbox-usage) <p> [Microsoft 365 Groups activity](https://docs.microsoft.com/microsoft-365/admin/activity-reports/office-365-groups)|In the [Microsoft 365 admin center](https://portal.office.com/adminportal/home), go to **Show all** (if necessary), click **Reports** \> **Usage**, and then select one of the reports on the page:<ul><li>**Email activity**</li><li>**Active users - Microsoft 365 services** \> **View more**:</li><ul><li> **Exchange**:</li><ul><li>**Email activity**</li><li>**Email app usage**</li><li>**Mailbox usage**</li></ul></li><li>**Office 365**:</li><ul><li>**Groups activity**</li></ul></ul></ul>|
|**Reports in the Security & Compliance Center**|[DLP reports](https://docs.microsoft.com/microsoft-365/compliance/view-the-dlp-reports)<sup>1</sup> <p> [Microsoft Defender for Office 365 reports](https://docs.microsoft.com/microsoft-365/security/office-365-security/view-reports-for-atp)<sup>2</sup> <p> [Email security reports](https://docs.microsoft.com/microsoft-365/security/office-365-security/view-email-security-reports) <p> [Mail flow reports](https://docs.microsoft.com/microsoft-365/security/office-365-security/view-mail-flow-reports)|In the [Security & Compliance Center](https://protection.office.com), go to **Reports** \> **Dashboard**. Select one of the available reports on the page. <p> |
|**Reports using Microsoft Graph**|Programmatically create the reports that are available in the Microsoft 365 admin center by using Microsoft Graph. For more information, see the following topics: <p>[Email activity reports](https://docs.microsoft.com/graph/api/resources/email-activity-reports) <p> [Email app usage reports](https://docs.microsoft.com/graph/api/resources/email-app-usage-reports) <p> [Mailbox usage reports](https://docs.microsoft.com/graph/api/resources/mailbox-usage-reports) <p> [Microsoft 365 groups activity reports](https://docs.microsoft.com/graph/api/resources/mailbox-usage-reports)|n/a|
|**Reports using reporting web services**|Programmatically create reports from the available Exchange Online PowerShell reporting cmdlets by using REST/ODATA2 query filtering.<sup>3</sup> <p> For more information, see [Reporting Web Services](https://docs.microsoft.com/previous-versions/office/developer/o365-enterprise-developers/jj984325(v=office.15)).|<https://reports.office365.com/ecp/reportingwebservice/reporting.svc>|
|**Message trace**|[Message trace in the Security & Compliance Center](https://docs.microsoft.com/microsoft-365/security/office-365-security/message-trace-scc) <p> [Message trace in the modern Exchange admin center](trace-an-email-message/message-trace-modern-eac.md)|In the [Security & Compliance Center](https://protection.office.com) or the [Exchange admin center](https://admin.exchange.microsoft.com), go to **Mail flow** \> **Message trace**.|
|**Audit logging**|[Search the audit log in the Security & Compliance Center](https://docs.microsoft.com/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance) <p> [Auditing reports in the classic Exchange admin center](../security-and-compliance/exchange-auditing-reports/exchange-auditing-reports.md)|In the [Security & Compliance Center](https://protection.office.com), go to **Search** \> **Audiy log search**. <p> In the [classic Exchange admin center](https://admin.protection.outlook.com/ecp/), go to **Compliance management** > **Auditing**.|
|

<sup>1</sup> DLP is only available in certain Exchange Online subscription plans. For information, see the **Data Loss Prevention** entries in the [Exchange Online Service Description](https://docs.microsoft.com/office365/servicedescriptions/exchange-online-service-description/exchange-online-service-description).

<sup>2</sup> Defender for Office 365 is available in Office 365 Enterprise E5, but you can also purchase Defender for Office 365 as an add-on to other subscription plans. For more information, see the [Microsoft Defender for Office 365 Service Description](https://docs.microsoft.com/office365/servicedescriptions/office-365-advanced-threat-protection-service-description).

<sup>3</sup> Many of the original reporting cmdlets in Exchange Online PowerShell have been deprecated (the cmdlets are available, but they don't return useful data). For a list of available and unavailable reporting cmdlets, see [Exchange reporting cmdlets](https://docs.microsoft.com/powershell/module/exchange/#reporting).

## Reporting and message trace data availability and latency

The following table describes when Exchange Online reporting and message trace data is available and for how long.

****

|Report type|Data available for (look back period)|Latency|
|---|---|---|
|Mailbox summary reports|60 days|Message data aggregation is mostly complete within 24-48 hours. Some minor incremental aggregated changes may occur for up to 5 days.|
|Mail protection summary reports|90 days|Message data aggregation is mostly complete within 24-48 hours. Some minor incremental aggregated changes may occur for up to 5 days.|
|Mail protection detail reports|90 days|For detail data that's less than 7 days old, data should appear within 24 hours but may not be complete until 48 hours. Some minor incremental changes may occur for up to 5 days. <br/> To view detail reports for messages that are greater than 7 days old, results may take up to a few hours.|
|Message trace data|90 days|When you run a message trace for messages that are less than 7 days old, the messages should appear within 5-30 minutes. <br/> When you run a message trace for messages that are greater than 7 days old, results may take up to a few hours.|
|

> [!NOTE]
> Data availability and latency doesn't depend on the user interface (it's the same in the admin centers as in PowerShell).
