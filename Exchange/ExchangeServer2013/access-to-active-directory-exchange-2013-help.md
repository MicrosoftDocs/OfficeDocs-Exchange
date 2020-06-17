---
title: 'Access to Active Directory: Exchange 2013 Help'
TOCTitle: Access to Active Directory
ms:assetid: 61080b45-8bce-4c23-b744-ed264d5f0b7d
ms:mtpsurl: https://technet.microsoft.com/library/Aa998561(v=EXCHG.150)
ms:contentKeyID: 49289272
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Access to Active Directory

_**Applies to:** Exchange Server 2013_

Microsoft Exchange Server 2013 stores all configuration and recipient information in the Active Directory directory service database. When a computer running Exchange 2013 requires information about recipients and information about the configuration of the Exchange organization, it must query Active Directory to access the information. Active Directory servers must be available for Exchange 2013 to function correctly.

This topic explains how Exchange 2013 stores and retrieves information in Active Directory so that you can plan access to Active Directory. This topic also discusses issues you should be aware of if you try to recover deleted Exchange 2013 Active Directory objects.

## Exchange information stored in Active Directory

The Active Directory database stores information in three types of logical partitions that are described in the following sections:

- Schema partition

- Configuration partition

- Domain partition

## Schema partition

The schema partition stores the following two types of information:

- **Schema classes** define all the types of objects that can be created and stored in Active Directory.

- **Schema attributes** define all the properties that can be used to describe the objects that are stored in Active Directory.

When you install the first Exchange 2013 server role in the forest or run the Active Directory preparation process, the Active Directory preparation process adds many classes and attributes to the Active Directory schema. The classes that are added to the schema are used to create Exchange-specific objects, such as agents and connectors. The attributes that are added to the schema are used to configure the Exchange-specific objects and the mail-enabled users and groups. These attributes include properties, such as Outlook Web App settings and Microsoft Exchange Unified Messaging (UM) settings. Every domain controller and global catalog server in the forest contains a complete replica of the schema partition.

For more information about schema modifications in Exchange 2013, see [Exchange 2013 Active Directory schema changes](exchange-2013-active-directory-schema-changes-exchange-2013-help.md).

## Configuration partition

The configuration partition stores information about the forest-wide configuration. This configuration information includes the configuration of Active Directory sites, Exchange global settings, transport settings, and mailbox policies. Each type of configuration information is stored in a container in the configuration partition. Exchange configuration information is stored in a subfolder under the configuration partition's Services container. The information that is stored in this container includes the following:

- Address lists

- Address book mailbox policies

- Administrative groups

- Client access settings

- Connections

- Mobile Mailbox Settings

- Global settings

- Monitoring Settings

- System policies

- Retention policies container

- Transport settings

Every domain controller and global catalog server in the forest contains a complete replica of the configuration partition.

## Domain partition

The domain partition stores information in default containers and in organizational units that are created by the Active Directory administrator. These containers hold the domain-specific objects. This data includes Exchange system objects and information about the computers, users, and groups in that domain. When Exchange 2013 is installed, Exchange updates the objects in this partition to support Exchange functionality. This functionality affects how recipient information is stored and accessed.

Each domain controller contains a complete replica of the domain partition for the domain for which it is authoritative. Every global catalog server in the forest contains a subset of the information in every domain partition in the forest.

## How Exchange 2013 accesses information in Active Directory

Exchange 2013 uses an Active Directory API to access information that is stored in Active Directory. The Microsoft Exchange Active Directory Topology service runs on all Exchange 2013 server roles. This service reads information from all Active Directory partitions. The data that is retrieved is cached and is used by Exchange 2013 servers to discover the Active Directory site location of all Exchange services in the organization.

For more information about topology and service discovery, see [Planning to use Active Directory sites for routing mail](planning-to-use-active-directory-sites-for-routing-mail-exchange-2013-help.md).

Exchange 2013 is an Active Directory site-aware application that prefers to communicate with the directory servers that are located in the same site as the Exchange server to optimize network traffic. Each Exchange 2013 organizational server role must communicate with Active Directory to retrieve information about recipients and information about the other Exchange 2013 server roles. The data that each server role obtains is described in the following sections.

By default, whenever an Exchange 2013 server starts, it binds to a randomly selected domain controller and global catalog server in its own site. You can view the selected directory servers by using the **Get-ExchangeServer** cmdlet in the Exchange Management Shell. You can also use the **Set-ExchangeServer** cmdlet to configure a static list of domain controllers to which an Exchange 2013 server should bind or a list of domain controllers that should be excluded.

> [!IMPORTANT]
> A Windows Server 2008 domain controller can be configured as a read-only directory server. This configuration is useful when you want to deploy a domain controller or global catalog server in a remote site for authentication and authorization purposes, but you don't want to allow administrators in that site to write changes to Active Directory. However, you can't deploy an Exchange 2013 server in any site that contains only read-only directory servers.

## Mailbox server role

The Mailbox server role stores configuration information about mailbox users and stores in Active Directory. Additionally, for Exchange 2013, the Mailbox server includes all the traditional server components found in Exchange 2010: the Client Access protocols, Transport service, Mailbox databases, and Unified Messaging. The Mailbox server handles all activity for the active mailboxes on that server.

## Client access server role

In Exchange 2013, the Client Access server provides authentication, limited redirection, and proxy services. The Client Access server itself doesn't do any data rendering. The Client Access server is a thin and stateless server. There is never anything queued or stored on the Client Access server. The Client Access server offers all the usual client access protocols: HTTP, POP and IMAP, and SMTP.

## Recovery of deleted Exchange objects

Active Directory Recycle Bin helps minimize directory service downtime by enhancing your ability to preserve and recover accidentally deleted Active Directory objects without restoring Active Directory data from backups, restarting Active Directory Domain Services (AD DS), or rebooting domain controllers.

The most important thing to understand about recovering deleted Exchange-related Active Directory objects is that Exchange objects don't exist in isolation. For example, when you mail-enable a user, several different policies and links are calculated for the user based on your current Exchange configuration. Two problems that may arise when you restore a deleted Exchange configuration or recipient object are:

- **Collisions**: Some Exchange attributes must be unique across a forest. For example, proxy (email) addresses must not be the same for two different users. Active Directory doesn't enforce proxy address uniqueness, Exchange administrative tools check for uniqueness. Exchange email address policies also automatically resolve possible conflicts in proxy address assignment based on deterministic rules. Therefore, it's possible to restore an Exchange user object and, as a result, create a collision with proxy addresses or other attributes that should be unique.

- **Misconfigurations**: Exchange has automated rules that assign various policies or settings. If you delete a recipient, and then change the rules or policies, restoring an Exchange user object may result in a user being assigned to the wrong policy (or even to a policy that no longer exists).

The following guidelines will help you minimize problems or issues when you recover deleted Exchange-related objects:

- If you deleted an Exchange configuration object using Exchange management tools, don't restore the object. Instead, create the object again using the Exchange management tools (Exchange admin center or Exchange Management Shell).

- If you deleted an Exchange configuration object without using the Exchange management tools, recover the object as soon as possible. The more administrative and configuration changes that have been made in the system since the deletion, the more likely it is that restoring the objects will result in misconfiguration.

- If you recover deleted Exchange recipients (contacts, users, or distribution groups), monitor closely for collisions and errors relating to the recovered objects. If Exchange policies or other configuration relating to recipients may have been modified since the deletion, re-apply current policies to the restored recipients to ensure that they're configured correctly.

## For more information

[Active Directory Recycle Bin Step-by-Step Guide](https://go.microsoft.com/fwlink/p/?linkid=178720)

[Introduction to Active Directory Administrative Center Enhancements (Level 100)](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/adac/introduction-to-active-directory-administrative-center-enhancements--level-100-)

[Advanced AD DS Management Using Active Directory Administrative Center (Level 200)](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/adac/advanced-ad-ds-management-using-active-directory-administrative-center--level-200-)
