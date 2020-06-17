---
title: 'Configure a federation trust: Exchange 2013 Help'
TOCTitle: Configure a federation trust
ms:assetid: 7c2b539f-faed-4374-8578-9f329ca628db
ms:mtpsurl: https://technet.microsoft.com/library/JJ657462(v=EXCHG.150)
ms:contentKeyID: 49289316
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure a federation trust

_**Applies to:** Exchange Server 2013_

A federation trust establishes a trust relationship between a Microsoft Exchange 2013 organization and the Azure Active Directory authentication system. By configuring a federation trust, you can configure federated sharing with other federated Exchange organizations to share calendar free/busy information among recipients. Federated sharing can be configured between two federated Exchange 2013 organizations or between a federated Exchange 2013 organization and federated Exchange 2010 organizations. You can also set up sharing with an Office 365 organization.

> [!NOTE]
> Creating a federation trust is one of several steps in setting up federated sharing in your Exchange organization. To review all the steps, see <A href="configure-federated-sharing-exchange-2013-help.md">Configure federated sharing</A>.

For additional management tasks related to federation, see [Federation procedures](federation-procedures-exchange-2013-help.md).

> [!IMPORTANT]
> This feature of Exchange Server 2013 isn't fully compatible with Office 365 operated by 21Vianet in China and some feature limitations may apply. For more information, see <A href="https://docs.microsoft.com/microsoft-365/admin/services-in-china/services-in-china">Learn about Office 365 operated by 21Vianet</A>.

## What do you need to know before you begin?

- Estimated time to complete: 30 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Federation and certificates" permissions entry in the [Exchange and Shell infrastructure permissions](exchange-and-shell-infrastructure-permissions-exchange-2013-help.md) topic.

- The domain used for establishing a federation trust should be resolvable from the Internet. This requires that the domain be registered with a domain registrar and the Domain Name System (DNS) zone for the domain to be hosted on a DNS server accessible from the Internet. If the organization receives Internet email for the domain, these requirements are already met.

- You will need to add a TXT record to your public DNS. Review the requirements for adding a TXT record with the organization that hosts your public DNS records.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

- Both Exchange organizations in a federated sharing relationship must use the same Azure AD authentication system for their federation trusts. This requirement applies when configuring federated sharing between two on-premises Exchange organizations or between an on-premises Exchange organization and an Exchange organization hosted by [Office 365](https://go.microsoft.com/fwlink/?linkid=392576).

- When you create a federation trust with the Azure AD authentication system for your Exchange 2013 organization, the federation trust will use the business instance of the Azure AD authentication system. However, other federated Exchange organizations with previous versions of Exchange and existing federation trusts may be using either the business or consumer instance of the Azure AD authentication system.

  The following Exchange organizations use the business instance of the Azure AD authentication system by default:

  - Exchange 2013 organizations by using the **Enable federation trust** wizard and self-signed certificates for a federation trust.

  - Exchange 2010 SP1 or later organizations by using the **New Federation Trust** wizard and self-signed certificates for a federation trust.

  - Exchange organizations hosted by Office 365, such as the Exchange Online.

    The following Exchange organizations use the consumer instance of the Azure AD authentication system by default:

  - Release to manufacturing (RTM) version of Exchange 2010 organizations using certificates issued by third-party certification authorities.

    We recommend that all Exchange organizations use the business instance of the Azure AD authentication system for federation trusts. Before configuring federated sharing between the two Exchange organizations, you need to verify which Azure AD authentication system instance each Exchange organization is using for any existing federation trusts. To determine which Azure AD authentication system instance an Exchange organization is using for an existing federation trust, run the following Shell command.

    ```powershell
    Get-FederationInformation -DomainName <hosted Exchange domain namespace>
    ```

    The business instance returns a value of `<uri:federation:MicrosoftOnline>` for the *TokenIssuerURIs* parameter.

    The consumer instance returns a value of `<uri:WindowsLiveID>` for the *TokenIssuerURIs* parameter.

    To configure federated sharing with an Exchange organization that has an existing federation trust that's using the business instance of the Azure AD authentication system, follow the steps in this topic. These steps are all you need to perform to create federation trusts that can be used to enable federated sharing between two Exchange 2013 organizations or between an Exchange 2013 organization and an Exchange 2010 organization that's already using the business instance of the Azure AD authentication system.

    To configure federated sharing between your Exchange 2013 organization and an Exchange organization that has an existing federation trust that's using the consumer instance of the Azure AD authentication system , the Exchange organization using the consumer instance should install Exchange 2010 SP2 or later, or upgrade to Exchange 2013. If you decide to install Exchange 2010 SP2 or later, use the **New Federation Trust** wizard to remove and re-create the existing federated domains and federation trusts. When the federation trusts are re-created, the business instance of the Azure AD authentication system will be used.

## Use the EAC to create and configure a federation trust

1. On an Exchange 2013 server in your on-premises organization, navigate to **Organization** \> **Sharing**.

2. Click **Enable** to start the **Enable federation trust** wizard.

3. After the wizard completes, click **Close**.

4. In the **Federation Trust** section of the **Sharing** tab, click **Modify**.

5. In **Sharing-Enabled Domains**, next to **Step 1**, click **Browse**.

6. In **Select Accepted Domains**, select the primary shared domain from the list, and then click **OK**.

    > [!NOTE]
    > The domain you select will be used to configure the OrgID for the federation trust. For more information about the OrgID, see <A href="federation-exchange-2013-help.md">Federation</A>.

7. Make a note of the federated domain proof that's generated for the primary shared domain. You'll use this string to create a TXT record on your public DNS server.

    > [!IMPORTANT]
    > The federated domain proof is a string of alphanumeric characters. To avoid input errors, we recommend that you copy the string from the EAC, and paste it into a text editor such as Notepad. You can then copy it from the text editor to the Clipboard, and then paste it into the <STRONG>Text</STRONG> field when creating the TXT record. If the TXT record is created by using an incorrect federated domain proof string, the Azure AD authentication system won't be able to verify proof of domain ownership, and you won't be able to add it to the federated organization identifier.

8. In **Step 2**, click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon") to add additional domains to the federated trust for email addresses that will be used by users in your organization that require federated sharing features. For example, if you have users that use a subdomain in their email address such as sales.contoso.com, you would add the sales.contoso.com domain to the federation trust.

    > [!NOTE]
    > A federated domain proof string will be created for each additional domain selected. You must create separate TXT records on your public DNS for each additional domain.

9. Using the federated domain proof strings created for each domain, create TXT records for each of these domains on your public DNS server. Depending on the update schedule of your public DNS host, replication of DNS changes may take 15 minutes or longer.

10. After the TXT records are created and replicated, click **Update**.

## Use the Shell to create and configure a federation trust

1. Run this command to create a unique subject key identifier for the federation trust certificate:

    ```powershell
    $ski = [System.Guid]::NewGuid().ToString("N")
    ```

2. Use this syntax to create a self-signed certificate for the federation trust:

    ```powershell
    New-ExchangeCertificate -FriendlyName "<Descriptive Name>" -DomainName <domain> -Services Federation -KeySize 2048 -PrivateKeyExportable $true -SubjectKeyIdentifier $ski
    ```

    This example creates a self-signed certificate for the federation trust with the Azure AD authentication system. The certificate uses the friendly name value Exchange Federated Sharing, and the domain value is retrieved from the **USERDNSDOMAIN** environment variable.

    ```powershell
    New-ExchangeCertificate -FriendlyName "Exchange Federated Sharing" -DomainName $env:USERDNSDOMAIN -Services Federation -KeySize 2048 -PrivateKeyExportable $true -SubjectKeyIdentifier $ski
    ```

3. To create the federation trust and automatically deploy the self-signed certificate that you created in the previous step to the Exchange servers in your organization, use this syntax:

    ```powershell
    Get-ExchangeCertificate | ?{$_.FriendlyName -eq "<FriendlyName>"} | New-FederationTrust -Name "<Descriptive Name>"
    ```

    This example creates the federation trust named Azure AD Authentication and deploys the self-signed certificate named Exchange Federated Sharing.

    ```powershell
    Get-ExchangeCertificate | ?{$_.FriendlyName -eq "Exchange Federated Sharing"} | New-FederationTrust -Name "Azure AD Authentication"
    ```

4. Use this syntax to return the proof of domain ownership TXT record that's required for any domain that you'll configure for the federation trust.

   ```powershell
   Get-FederatedDomainProof -DomainName <domain>
   ```

   This example returns the proof of domain ownership TXT record that's required for the primary shared domain contoso.com.

   ```powershell
   Get-FederatedDomainProof -DomainName contoso.com
   ```

   **Notes**:

   - Each domain or subdomain that's configured for the federation trust requires a proof of domain ownership TXT record, so you might need to run this command multiple times using different *DomainName* values.

   - We recommend that you copy the domain proof string by right-clicking in the Shell, selecting **Mark**, selecting the **Proof** value, and then pressing Enter so you can use it when you create the TXT record. If you create the TXT record with an incorrect federated domain proof string, the Azure AD authentication system can't verify your ownership of the domain, and you won't be able to add it to the federated organization identifier.

5. Using the information from the previous step, create TXT records on your public DNS server in every domain that will be included in the federation trust. Depending on the update schedule of your public DNS host, replication of DNS changes may take 15 minutes or longer. Continue after you've verified that the new TXT records are available.

    > [!IMPORTANT]
    > The TXT record should be created for every domain that is being federated/shared. If this is a hybrid environment then all accepted domains that are validated in Exchange Online should have TXT records created.

6. Run this command to retrieve the metadata and certificate from Azure AD:

   ```powershell
   Set-FederationTrust -RefreshMetadata -Identity "Azure AD authentication"
   ```

7. Use this syntax to configure the primary shared domain for the federation trust that you created in Step 3. The domain that you specify will be used to configure the organization identifier (OrgID) for the federation trust. For more information about the OrgID, see [Federated organization identifier](federation-exchange-2013-help.md).

   ```powershell
   Set-FederatedOrganizationIdentifier -DelegationFederationTrust "<Federation Trust Name>" -AccountNamespace <Accepted Domain> -Enabled $true
   ```

   This example configures the accepted domain contoso.com as the primary shared domain for the federation trust named Azure AD Authentication.

   ```powershell
   Set-FederatedOrganizationIdentifier -DelegationFederationTrust "Azure AD authentication" -AccountNamespace contoso.com -Enabled $true
   ```

8. To add other domains to the federation trust, use this syntax:

   ```powershell
   Add-FederatedDomain -DomainName <AdditionalDomain>
   ```

   This examples adds the subdomain sales.contoso.com to the federated trust, because users with email addresses in the sales.contoso.com domain require federated sharing features.

   ```powershell
   Add-FederatedDomain -DomainName sales.contoso.com
   ```

   Remember, any domain or subdomain that you add to the federation trust requires a proof of domain ownership TXT record,

For detailed syntax and parameter information, see [New-ExchangeCertificate](https://docs.microsoft.com/powershell/module/exchange/New-ExchangeCertificate), [New-FederationTrust](https://docs.microsoft.com/powershell/module/exchange/New-FederationTrust), [Get-FederatedDomainProof](https://docs.microsoft.com/powershell/module/exchange/Get-FederatedDomainProof), [Set-FederationTrust](https://docs.microsoft.com/powershell/module/exchange/Set-FederationTrust), [Set-FederatedOrganizationIdentifier](https://docs.microsoft.com/powershell/module/exchange/Set-FederatedOrganizationIdentifier), and [Add-FederatedDomain](https://docs.microsoft.com/powershell/module/exchange/Add-FederatedDomain).

## How do you know this worked?

The successful completion of the **Enable federation trust** and **Sharing-Enabled Domains** wizards will be your first indication that the federation trust was configured as expected.

To further verify that you have successfully created and configured the federation trust, do the following:

1. Run the following Shell command to verify the federation trust information.

   ```powershell
   Get-FederationTrust | Format-List
   ```

2. Replace *\<PrimarySharedDomain\>* with your primary shared domain, and run the following Shell command to verify that federation information can be retrieved from your organization.

   ```powershell
   Get-FederationInformation -DomainName <PrimarySharedDomain>
   ```

For detailed syntax and parameter information, see [Get-FederationTrust](https://docs.microsoft.com/powershell/module/exchange/Get-FederationTrust) and [Get-FederationInformation](https://docs.microsoft.com/powershell/module/exchange/Get-FederationInformation).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).
