---
title: 'Disjoint namespace scenarios: Exchange 2013 Help'
TOCTitle: Disjoint namespace scenarios
ms:assetid: 90101d49-6f45-44be-8a93-eeb2c8283e3b
ms:mtpsurl: https://technet.microsoft.com/library/Bb676377(v=EXCHG.150)
ms:contentKeyID: 49289351
ms.reviewer:
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Disjoint namespace scenarios

_**Applies to:** Exchange Server 2013_

This topic provides information about the concept of disjoint namespaces and the supported scenarios for deploying Microsoft Exchange 2013 in a domain that has a disjoint namespace.

## DNS and NetBIOS domain names

First, some background. Every computer that is on the Internet has a Domain Name System (DNS) name. This is also known as the *machine name* or *host name*. Every computer running the Windows operating system with networking capabilities also has a NetBIOS name.

A computer running Windows in an Active Directory domain has both a DNS domain name and a NetBIOS domain name, as follows:

- **DNS domain name**: The DNS domain name consists of one or more subdomains separated by a dot (**.**) and is terminated by a top-level domain name. For example, in the DNS domain name corp.contoso.com, the subdomains are corp and contoso and the top-level domain name is com.

- **NetBIOS domain name**: Typically, the NetBIOS domain name is the subdomain of the DNS domain name. For example, if the DNS domain name is contoso.com, the NetBIOS domain name is contoso. If the DNS domain name is corp.contoso.com, the NetBIOS domain name is corp.

> [!NOTE]
> To find DNS and NetBIOS information for computers running Windows Server 2008, see View DNS and NetBIOS name-related information of a computer running Windows Server 2008.

A computer in an Active Directory domain also has a primary DNS suffix and can have additional DNS suffixes. By default, the primary DNS suffix is the same as the DNS domain name. For detailed steps about how to change the primary DNS suffix, see the procedures later in this topic.

You define the DNS domain name and NetBIOS domain name of an Active Directory domain when you configure the first domain controller in the domain. For more information about configuring domain controllers, see [Domain Controller Roles](https://go.microsoft.com/fwlink/p/?linkid=268367) and [Active Directory Domain Services Overview](https://go.microsoft.com/fwlink/p/?linkid=268366).

## Disjoint namespaces

In most domain topologies, the primary DNS suffix of the computers in the domain is the same as the DNS domain name.

In some cases, you may require these namespaces to be different. This is called a *disjoint namespace*. For example, a merger or acquisition may cause you to have a topology with a disjoint namespace. In addition, if DNS management in your company is split between administrators who manage Active Directory and administrators who manage networks, you may need to have a topology with a disjoint namespace.

A disjoint namespace scenario is one in which the primary DNS suffix of a computer doesn't match the DNS domain name where that computer resides. The computer with the primary DNS suffix that doesn't match is said to be *disjoint*. Another disjoint namespace scenario occurs if the NetBIOS domain name of a domain controller doesn't match the DNS domain name.

## Exchange 2013 and disjoint namespaces

Exchange 2013 supports the following three scenarios for deploying Exchange in a domain that has a disjoint namespace:

- **Primary DNS suffix and DNS domain name are different**: The primary DNS suffix of the domain controller isn't the same as the DNS domain name. Computers that are members of the domain can be either disjoint or not disjoint.

- **Member computer is disjoint**: A member computer in an Active Directory domain is disjoint, even though the domain controller is not disjoint.

- **NetBIOS name of domain controller differs from subdomain of its DNS domain name**: The NetBIOS domain name of the domain controller isn't the same as the subdomain of the DNS domain name of that domain controller.

These scenarios are detailed in the following sections.

> [!NOTE]
> It's supported to run Exchange 2013 in the disjoint namespace scenarios described in this topic. However, if you have a disjoint namespace scenario that isn't one of the scenarios described in this topic, you must work with Microsoft Services to deploy Exchange 2013. For more information, see <A href="https://go.microsoft.com/fwlink/p/?linkid=94845">Microsoft Services</A>.

## Scenario: Primary DNS suffix and DNS domain name are different

In this scenario, the primary DNS suffix of the domain controller isn't the same as the DNS domain name. The domain controller is disjoint in this scenario. Computers that are members of the domain, including Exchange servers and Microsoft Outlook client computers, can have a primary DNS suffix that either matches the primary DNS suffix of the domain controller or matches the DNS domain name.

## Scenario: Member computer is disjoint

In this scenario, the primary DNS suffix of a member computer on which Exchange 2013 is installed isn't the same as the DNS domain name, even though the primary DNS suffix of the domain controller is the same as the DNS domain name. In this scenario, you have a domain controller that isn't disjoint and a member computer that is disjoint. Member computers that are running Outlook can have a primary DNS suffix that either matches the primary DNS suffix of the disjoint Exchange server or matches the DNS domain name.

## Scenario: NetBIOS name of domain controller differs from subdomain of its DNS domain name

In this scenario, the NetBIOS domain name of the domain controller isn't the same as the DNS domain name of the same domain controller.

**NetBIOS domain name doesn't match DNS domain name**

![NetBIOS domain name does not match DNS domain name](images/Bb676377.1ee18cb6-0296-4875-b572-0ddf33f65f7c(EXCHG.150).gif "NetBIOS domain name does not match DNS domain name")

## Allow Exchange 2013 servers to access domain controllers that are disjoint

To allow Exchange 2013 servers to access domain controllers that are disjoint, you must modify the **msDS-AllowedDNSSuffixes** Active Directory attribute on the domain object container. You must add both of the DNS suffixes to the attribute. For detailed steps about how to modify the attribute, see [The computer's primary DNS suffix does not match the FQDN of the domain where it resides](https://docs.microsoft.com/previous-versions/office/exchange-server-analyzer/aa998420(v=exchg.80)).

In addition, to make sure that the DNS suffix search list contains all DNS namespaces that are deployed within the organization, you must configure the search list for each computer in the domain that is disjoint. The list of namespaces should include not only the primary DNS suffix of the domain controller and the DNS domain name, but also any additional namespaces for other servers with which Exchange may interoperate (such as monitoring servers or servers for third-party applications). You can do this by setting Group Policy for the domain. For more information about Group Policy, see [Group Policy Overview](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831791(v=ws.11)).

For detailed steps about how to configure the DNS suffix search list Group Policy, see [Configure the DNS suffix search list for a disjoint namespace](configure-the-dns-suffix-search-list-for-a-disjoint-namespace-exchange-2013-help.md).

## View DNS and NetBIOS name-related information of a computer running Windows Server 2008

1. Click **Start**, right-click **Computer**, and then click **Properties**.

2. In **System**, the DNS host name and primary DNS suffix are displayed under **Computer name, domain, and workgroup settings** next to **Full computer name**. The DNS domain name is displayed next to **Domain**.

3. Click **Change settings**.

4. In **System Properties**, on the **Computer Name** tab, click **Change**.

5. In **Computer Name/Domain Changes**, click **More**. The primary DNS suffix is displayed under **Primary DNS suffix of this computer**. The NetBIOS computer name is displayed under **NetBIOS computer name**.

    To change the primary DNS suffix, type the new primary DNS suffix under **Primary DNS suffix of this computer**, and then click **OK**.

6. From a Command Prompt window, type **set**. The variable USERDNSDOMAIN displays the DNS domain name. The variable USERDOMAIN displays the NetBIOS domain name.
