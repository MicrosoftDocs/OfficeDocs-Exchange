---
title: 'IIS Logs and Log Parser Studio Reports: Exchange 2013 Help'
TOCTitle: IIS Logs and Log Parser Studio Reports
ms:assetid: 01fa67d4-dc02-4c5f-93af-6da7b97d282f
ms:mtpsurl: https://technet.microsoft.com/library/Dn904092(v=EXCHG.150)
ms:contentKeyID: 63917935
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# IIS Logs and Log Parser Studio Reports

_**Applies to:** Exchange Server 2013_

## Analyzing Log Parser Studio Reports

Log Parser Studio is a utility that allows you to search through and create reports from several types of log files, including those for Internet Information Services (IIS). It builds on top of Log Parser 2.2 and has a full user interface for easy creation and management of related SQL queries.

[Download Log Parser Studio](https://gallery.technet.microsoft.com/Log-Parser-Studio-cd458765) and then review [Introducing: Log Parser Studio](https://techcommunity.microsoft.com/t5/exchange-team-blog/introducing-log-parser-studio/ba-p/601131).

Remember that in Exchange 2013, all traffic has to go through IIS. This means analyzing IIS logs is the best way to get a complete picture of the number of connections that are hitting a server, of protocol-specific information about the connections, and of the users who are most impacting performance. Over twenty new reports have been developed for Log Parser Studio, for the purpose of troubleshooting Exchange 2013 performance issues.

## Log Parser Studio Reporting for Exchange 2013 Performance Issues

To gain a comprehensive understanding of overall load in your Exchange 2013 environment, use the following reporting and compare the numbers against each server.

A .zip file containing the Log Parser Studio reports listed here, and additional troubleshooting-related reports, can be [downloaded from here](https://gallery.technet.microsoft.com/Log-Parser-Studio-Report-f17f6b8b).

- **IIS: Requests Per Hour**. Feed in IIS logs from either the Default Web Site (W3SVC1 directory) or the Backend Website (W3SVC2 directory), but not both at the same time.

- **ACTIVESYNC\_WP: Clients by percent**. Calculates all ActiveSync requests broken down by user-agent and percentage of each client to the total number of requests.

- **ACTIVESYNC\_WP: Requests per hour (CSV)**. Lists the ActiveSync requests per hour and sends the results to a CSV file.

- **ACTIVESYNC\_WP: Requests per user (CSV)**. Lists ActiveSync requests per user and sends the results to a CSV file.

- **ACTIVESYNC\_WP: Requests per user (Top 10k)**. Lists ActiveSync requests per user for the top 10,000 users.

- **ACTIVESYNC\_WP: Top Talkers (CSV)**. Lists the top ActiveSync clients from highest to lowest request count and sends the result to a CSV file.

- **EWS\_WP: Clients by percent**. Calculates all EWS requests broken down by user-agent and percentage of each client to the total number of requests.

- **EWS\_WP: Requests per hour (CSV)**. Lists the total number of EWS requests per hour.

- **EWS\_WP: Requests per user (CSV)**. Lists EWS requests per user and sends the results to a CSV file.

- **EWS\_WP: Requests per user (Top 10k)**. Lists EWS requests per user for the top 10,000 users.

- **EWS\_WP: Top Talkers (CSV)**. Lists the top EWS clients from highest to lowest request count.

- **OLA\_WP: Errors, per user, per hour, per day**. Outlook Anywhere users by number of requests.

- **OLA\_WP: Requests per hour**. Lists the Outlook Anywhere requests per hour.

- **OLA\_WP: Requests per hour, per user**. Lists the Outlook Anywhere requests per hour, per user.

- **OLA\_WP: Requests per user (CSV)**. Lists Outlook Anywhere requests per user.

- **OLA\_WP: Requests per user (Top 10k)**. Lists Outlook Anywhere requests per user for the top 10,000 users.

- **OLA\_WP: Top Talkers**. Lists the top Outlook Anywhere clients from highest to lowest request count.

- **OWA\_WP: Clients by percent**. Calculates all OWA requests broken down by user-agent and percentage of each client to the total number of requests.

- **OWA\_WP: Requests per hour (CSV)**. Lists the OWA requests per hour and sends the results to a CSV file.

- **OWA\_WP: Requests per user (CSV)**. Lists OWA requests per user and sends the results to a CSV file.

- **OWA\_WP: Requests per user (Top 10k)**. Lists OWA requests per user for the top 10,000 users.

- **OWA\_WP: Top Talkers (CSV)**. Lists the top OWA clients from highest to lowest request count and sends the result to a CSV file.
