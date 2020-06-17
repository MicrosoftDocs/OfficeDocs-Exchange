---
title: 'Manage a federation trust: Exchange 2013 Help'
TOCTitle: Manage a federation trust
ms:assetid: 0439839f-2052-4bc9-9d30-aa6e7d51b733
ms:mtpsurl: https://technet.microsoft.com/library/JJ673036(v=EXCHG.150)
ms:contentKeyID: 49289152
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Manage a federation trust

_**Applies to:** Exchange Server 2013_

A federation trust establishes a trust relationship between a Microsoft Exchange 2013 organization and the Azure Active Directory authentication system and supports federated sharing with other federated Exchange organizations. Normally, you shouldn't have to manage or modify the federation trust after it's created. However, there may be circumstances that require adding or removing federated domains or resetting the domain used to configure the organization identifier (OrgID) for the federation trust.

> [!NOTE]
> Modifying an existing federation trust, especially the primary shared domain used to define the OrgID, can disrupt federated sharing between federated Exchange organizations or for hybrid deployments with Microsoft 365 or Office 365 organizations.

For additional management tasks related to Federation, see [Federation procedures](federation-procedures-exchange-2013-help.md).

> [!IMPORTANT]
> This feature of Exchange Server 2013 isn't fully compatible with Office 365 operated by 21Vianet in China and some feature limitations may apply. For more information, see <A href="https://docs.microsoft.com/microsoft-365/admin/services-in-china/services-in-china">Learn about Office 365 operated by 21Vianet</A>.

## What do you need to know before you begin?

- Estimated time to complete: 30 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the *Federation and certificates* permissions entry in the [Exchange and Shell infrastructure permissions](exchange-and-shell-infrastructure-permissions-exchange-2013-help.md) topic.

- You will need to add a TXT record to your public DNS for each new federated domain added to the federation trust. Review the requirements for adding a TXT record with the organization that hosts your public DNS records.

- For the purposes of this topic, an existing federation trust was configured with the following settings:

  - **Contoso.com** is the primary shared domain for the federation trust. (This domain will not be changed.)

  - The federated domains **service.contoso.com** and **sales.contoso.com** are included in the existing federation trust.

  - **Marketing.contoso.com** is an accepted domain in the Exchange organization.

- This topic also covers other federation management tasks, such as viewing and managing certificates used for the federation trust and viewing federation trust parameter information in the Shell.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

## Use the EAC to manage a federation trust

1. On an Exchange 2013 server in your on-premises organization, navigate to the **Organization** \> **Sharing**.

2. In the **Federation Trust** section, click **Modify**.

3. In **Sharing-Enabled Domains**, skip **Step 1** because the primary sharing domain isn't changing.

4. In **Step 2**, select the **service.contoso.com** domain and then click **Remove** ![Remove icon](images/Dd362328.479b6ced-8d64-4277-a725-f17fea202b28(EXCHG.150).gif "Remove icon") to remove the domain from the federated trust.

5. In **Step 2**, click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

6. In **Select Accepted Domains**, select **marketing.contoso.com** from the list of accepted domains, and then click **OK** to add the domain to the federated trust.

    > [!IMPORTANT]
    > A federated domain proof string will be created for the <STRONG>marketing.contoso.com</STRONG> domain. You must create separate TXT record on your public DNS for this domain.

7. Using the federated domain proof string created for the **marketing.contoso.com** domain, create a TXT record on your public DNS server. Depending on the update schedule of your public DNS host, replication of DNS changes may take 15 minutes or longer.

8. After the TXT record is created and replicated, click **Update**.

## Use the Shell to manage a federation trust

1. This example removes the service.contoso.com domain from the federation trust.

    ```powershell
    Remove-FederatedDomain -DomainName service.contoso.com
    ```

2. This example adds the marketing.contoso.com domain to the federation trust.

    ```powershell
    Add-FederatedDomain -DomainName marketing.contoso.com
    ```

For detailed syntax and parameter information, see [Remove-FederatedDomain](https://docs.microsoft.com/powershell/module/exchange/Remove-FederatedDomain) and [Add-FederatedDomain](https://docs.microsoft.com/powershell/module/exchange/Add-FederatedDomain).

Run the following Shell commands to manage other aspects of a federation trust:

1. **View the federated OrgID and federated domains**

    This example displays the Exchange organization's federated OrgID and related information, including federated domains and status.

    ```powershell
    Get-FederatedOrganizationIdentifier
    ```

2. **View federation trust certificates**

    This example displays the previous, current, and next certificates used by the federation trust "Azure AD authentication".

    ```powershell
    Get-FederationTrust "Azure AD authentication" | Select Org*certificate
    ```

3. **Check federation certificates status**

    This example displays the state of federation certificates on all Mailbox and Client Access servers in the organization.

    ```powershell
    Test-FederationTrustCertificate
    ```

4. **Configure the federation trust to use a certificate as the next certificate**

    This example configures the federation trust "Azure AD authentication" to use the certificate with the provided thumbprint as the next certificate. After the certificate is deployed to all Exchange servers in the organization, you can use the *PublishCertificate* switch to configure the federation trust to use this certificate as the current certificate.

    ```powershell
    Set-FederationTrust "Azure AD authentication" -Thumbprint AC00F35CBA8359953F4126E0984B5CCAFA2F4F17
    ```

5. **Configure the federation trust to use the next certificate as the current certificate**

    This example configures the federation trust Azure AD authentication to use the next certificate as the current certificate and publishes it to the Azure AD authentication system.

    ```powershell
    Set-FederationTrust "Azure AD authentication" -PublishFederationCertificate
    ```

    > [!WARNING]
    > Before configuring the federation trust to use the next certificate as the current federation certificate, make sure that the certificate is deployed on all Exchange servers in your organization. Use the <A href="https://docs.microsoft.com/powershell/module/exchange/Test-FederationTrustCertificate">Test-FederationTrustCertificate</A> cmdlet to check the deployment status of the certificate.

6. **Refresh federation metadata and certificate from the Azure AD authentication system**

    This example refreshes the federation metadata and certificate of the Azure AD authentication system for the federation trust Azure AD authentication.

    ```powershell
    Set-FederationTrust "Azure AD authentication" -RefreshMetadata
    ```

For detailed syntax and parameter information, see the following topics:

- [Get-FederatedOrganizationIdentifier](https://docs.microsoft.com/powershell/module/exchange/Get-FederatedOrganizationIdentifier)

- [Get-FederationTrust](https://docs.microsoft.com/powershell/module/exchange/Get-FederationTrust)

- [Test-FederationTrustCertificate](https://docs.microsoft.com/powershell/module/exchange/Test-FederationTrustCertificate)

- [Set-FederationTrust](https://docs.microsoft.com/powershell/module/exchange/Set-FederationTrust)

>[!NOTE]
>There are additional considerations if the tenant is hosted in the Office 365 U.S. Government GCC High or DoD environment. In these environments, you must run the **Set-FederationTrust** cmdlet in the on-premises Exchange environment with a different value for the *MetadataUrl* parameter. See [Set-FederationTrust](https://docs.microsoft.com/powershell/module/exchange/Set-FederationTrust) for more information.

## How do you know this worked?

The successful completion of the **Sharing-enabled domains** wizard is your first indication that you configured the federation trust as expected.

To further verify success, do the following:

1. Run the following Shell command to verify the federation trust information.

    ```powershell
    Get-FederationTrust | format-list
    ```

2. Run the following Shell command to verify that federation information can be retrieved from your organization. For example, verify that the sales.contoso.com and marketing.contoso.com domains are returned in the *DomainNames* parameter.

    ```powershell
    Get-FederationInformation -DomainName <your primary sharing domain>
    ```

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).
