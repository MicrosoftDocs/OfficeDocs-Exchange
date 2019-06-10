---
localization_priority: Normal
description: 'Summary: Learn about managed availability and workload management in Exchange Server.'
ms.topic: article
author: chrisda
ms.author: chrisda
ms.assetid: 9d1fdec8-8273-4c71-88f1-b4edfd542c4f
ms.date: 7/9/2018
ms.reviewer: 
title: Server health and performance in Exchange Server
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Server health and performance in Exchange Server

Understanding server health and performance is critical to designing and maintaining a high-performance messaging infrastructure. Exchange 2016 and Exchange 2019 continue the features that were introduced in Exchange 2013 to help you manage server health and performance.

## Managed availability

 *Managed availability* provides built-in monitoring and recovery actions that preserve the end-user experience. Managed availability is made of two processes: the Exchange Health Manager Service (MSExchangeHMHost.exe) and the Exchange Health Manager Worker process (MSExchangeHMWorker.exe), and the following components:

- **Probe engine**: The *probe engine* takes measurements on the server.

- **Monitoring probe engine**: The *monitoring probe engine* stores the business logic about what constitutes a healthy state. Like a pattern recognition engine, the monitoring probe engine looks for patterns and measurements that differ from a healthy state, and then evaluates whether a component or feature is unhealthy.

- **Responder engine**: When the *responder engine* is alerted about an unhealthy component, it first tries to recover that component. Managed availability enables multi-stage recovery actions. The first attempt may be to restart the application pool, the second attempt may be to restart the corresponding service, and the third attempt may be to restart the server. And, the final attempt may be to put the server offline, so that it no longer accepts traffic. If all of these actions fail, an alert is sent to the help desk.

For more information about managed availability, see [Managed availability](../high-availability/managed-availability/managed-availability.md).

## Workload management

Workload management is made of these components:

- *User workload management* is the new name for the user throttling features that were introduced in Exchange 2010. You can customize these setting based on the needs of your environment.

- *System workload management* automatically throttles specific Exchange workloads by monitoring the health of key server resources. These settings should be customized only under the direction of Microsoft Customer Service and Support.

For more information about user workload management, see [User workload management in Exchange Server](workload-management.md).
