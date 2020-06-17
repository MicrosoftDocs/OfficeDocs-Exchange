---
title: 'Configure OAuth authentication between Exchange and Exchange Online'
TOCTitle: Configure OAuth authentication between Exchange and Exchange Online organizations
ms:assetid: f703e153-98e2-4268-8a6e-07a86b0a1d22
ms:mtpsurl: https://technet.microsoft.com/library/Dn594521(v=EXCHG.150)
ms:contentKeyID: 61200240
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure OAuth authentication between Exchange and Exchange Online organizations

_**Applies to:** Exchange Server 2013_

Exchange 2013-only hybrid deployments configure OAuth authentication when using the Hybrid Configuration Wizard. For mixed Exchange 2013/2010 and Exchange 2013/2007 hybrid deployments, the new hybrid deployment OAuth-based authentication connection between Office 365 and on-premises Exchange organizations isn't configured by the Hybrid Configuration wizard. These deployments continue to use the federation trust process by default. However, certain Exchange 2013 features are only fully available across your organization by using the new Exchange OAuth authentication protocol.

The new Exchange OAuth authentication process currently enables the following Exchange features:

- Message Records Management (MRM)

- Exchange In-place eDiscovery

- Exchange In-place Archiving

We recommend that all mixed Exchange organizations that implement a hybrid deployment with Exchange 2013 and Exchange Online configure Exchange OAuth authentication after configuring their hybrid deployment with the Hybrid Configuration Wizard.

> [!IMPORTANT]
> If your on-premises organization is running only Exchange 2013 servers with Cumulative Update 5 or later installed, run the Hybrid Deployment Wizard instead of performing the steps in this topic.<BR>This feature of Exchange Server 2013 isn't fully compatible with Office 365 operated by 21Vianet in China and some feature limitations may apply. For more information, see <A href="https://docs.microsoft.com/microsoft-365/admin/services-in-china/services-in-china">Learn about Office 365 operated by 21Vianet</A>.

## What do you need to know before you begin?

- Estimated time to complete this task: 15 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Federation and certificates" permissions entry in the [Exchange and Shell infrastructure permissions](exchange-and-shell-infrastructure-permissions-exchange-2013-help.md) topic.

- Completed configuration of your hybrid deployment using the Hybrid Deployment Wizard. For more information, see [Exchange Server Hybrid Deployments](https://docs.microsoft.com/exchange/exchange-hybrid).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## How do you configure OAuth authentication between your on-premises Exchange and Exchange Online organizations?

### Step 1: Create the authorization server objects for your Exchange Online organization

For this procedure, you have to specify a verified domain for your Exchange Online organization and the Exchange Online Tenant Name. The first domain should be the same domain used as the primary SMTP domain used for the cloud-based email accounts. This domain is referred as *\<your verified domain\>* in the following procedure.  The second domain which is your actual tenant name like contoso.onmicrosoft.com is referred to as *\<your tenant domain\>*.

Run the following command in the Exchange Management Shell (Exchange PowerShell) in your on-premises Exchange organization:

```powershell
New-AuthServer -Name "WindowsAzureACS" -AuthMetadataUrl "https://accounts.accesscontrol.windows.net/<your verified domain>/metadata/json/1"
New-AuthServer -Name "evoSTS" -Type AzureAD -AuthMetadataUrl "https://login.windows.net/<your tenant domain>/federationmetadata/2007-06/federationmetadata.xml"
```

### Step 2: Enable the partner application for your Exchange Online organization

Run the following command in the Exchange PowerShell in your on-premises Exchange organization.

```powershell
Get-PartnerApplication |  ?{$_.ApplicationIdentifier -eq "00000002-0000-0ff1-ce00-000000000000" -and $_.Realm -eq ""} | Set-PartnerApplication -Enabled $true
```

### Step 3: Export the on-premises authorization certificate

In this step, you have to run a PowerShell script to export the on-premises authorization certificate, which is then imported to your Exchange Online organization in the next step.

1. Save the following text to a PowerShell script file named, for example, **ExportAuthCert.ps1**.

   ```powershell
   $thumbprint = (Get-AuthConfig).CurrentCertificateThumbprint
   if((test-path $env:SYSTEMDRIVE\OAuthConfig) -eq $false)
   {
      md $env:SYSTEMDRIVE\OAuthConfig
   }
   cd $env:SYSTEMDRIVE\OAuthConfig
   $oAuthCert = (dir Cert:\LocalMachine\My) | where {$_.Thumbprint -match $thumbprint}
   $certType = [System.Security.Cryptography.X509Certificates.X509ContentType]::Cert
   $certBytes = $oAuthCert.Export($certType)
   $CertFile = "$env:SYSTEMDRIVE\OAuthConfig\OAuthCert.cer"
   [System.IO.File]::WriteAllBytes($CertFile, $certBytes)
   ```

2. In Exchange PowerShell in your on-premises Exchange organization, run the PowerShell script that you created in the previous step. For example:

   ```powershell
   .\ExportAuthCert.ps1
   ```

### Step 4: Upload the on-premises authorization certificate to Azure Active Directory Access Control Service (ACS)

Next, you have to use Windows PowerShell to upload the on-premises authorization certificate that you exported in the previous step to Azure Active Directory Access Control Services (ACS). To do this, the Azure Active Directory Module for Windows PowerShell cmdlets has to be installed. If it's not installed, go to <https://aka.ms/aadposh> to install the Azure Active Directory Module for Windows PowerShell. Complete the following steps after the Azure Active Directory Module for Windows PowerShell is installed.

1. Click the **Azure Active Directory Module for Windows PowerShell** shortcut to open a Windows PowerShell workspace that has the Azure AD cmdlets installed. All commands in this step will be run using the Windows PowerShell for Azure Active Directory console.

2. Save the following text to a PowerShell script file named, for example, **UploadAuthCert.ps1**.

   ```powershell
   Connect-MsolService
   $CertFile = "$env:SYSTEMDRIVE\OAuthConfig\OAuthCert.cer"
   $objFSO = New-Object -ComObject Scripting.FileSystemObject
   $CertFile = $objFSO.GetAbsolutePathName($CertFile)
   $cer = New-Object System.Security.Cryptography.X509Certificates.X509Certificate
   $cer.Import($CertFile)
   $binCert = $cer.GetRawCertData()
   $credValue = [System.Convert]::ToBase64String($binCert)
   $ServiceName = "00000002-0000-0ff1-ce00-000000000000"
   $p = Get-MsolServicePrincipal -ServicePrincipalName $ServiceName
   New-MsolServicePrincipalCredential -AppPrincipalId $p.AppPrincipalId -Type asymmetric -Usage Verify -Value $credValue
   ```

3. Run the PowerShell script that you created in the previous step. For example:

   ```powershell
   .\UploadAuthCert.ps1
   ```

4. After you start the script, a credentials dialog box is displayed. Enter the credentials for the tenant administrator account in your Microsoft Online Azure AD organization. After running the script, leave the Windows PowerShell for Azure AD session open. You will use this to run a PowerShell script in the next step.

### Step 5: Register all hostname authorities for your internal and external on-premises Exchange HTTP endpoints with Azure Active Directory

You have to run the script in this step for each endpoint in your on-premises Exchange organization that is publically accessible (Internal and External URLs if you are going to setup Hybrid Modern Authentication). For example, assume that Exchange is externally available on **https://mail.contoso.com/ews/exchange.asmx**. In this case the service principal name of: https://mail.contoso.com would be used.  There isn't a limit for registering additional external hostname authorities.

If you are not sure of the Exchange endpoints in your on-premises Exchange organization, you can get a list of the external configured Web services endpoints by running the following command in Exchange PowerShell in your on-premises Exchange organization:

```powershell
Get-MapiVirtualDirectory | FL server,*url*
Get-WebServicesVirtualDirectory | FL server,*url*
Get-OABVirtualDirectory | FL server,*url*
```

> [!NOTE]
> Successfully running the following script requires that the Windows PowerShell for Azure Active Directory is connected to your Microsoft Online Azure AD tenant, as explained in step 4 in the previous section.

1. Save the following text to a PowerShell script file named, for example, **RegisterEndpoints.ps1**. This example uses a contoso.com. Replace https://mail.contoso.com/ & https://autodiscover.contoso.com/ with the appropriate hostname authority for your on-premises Exchange organization.

   ```powershell
   $ServiceName = "00000002-0000-0ff1-ce00-000000000000";
   $x = Get-MsolServicePrincipal -AppPrincipalId $ServiceName;
   $x.ServicePrincipalnames.Add("https://mail.contoso.com/");
   $x.ServicePrincipalnames.Add("https://autodiscover.contoso.com/");
   Set-MSOLServicePrincipal -AppPrincipalId $ServiceName -ServicePrincipalNames $x.ServicePrincipalNames;
   ```

2. In Windows PowerShell for Azure Active Directory, run the Windows PowerShell script that you created in the previous step. For example:

   ```powershell
   .\RegisterEndpoints.ps1
   ```

3. To verify that all the records were added we need to run the following. We’re looking for https://namespace entries for all the URL’s, not 00000002-0000-0ff1-ce00-000000000000/namespace entries.

   ```powershell
   Get-MsolServicePrincipal -AppPrincipalId 00000002-0000-0ff1-ce00-000000000000 | select -ExpandProperty ServicePrincipalNames
   ```

### Step 6: Create an IntraOrganizationConnector from your on-premises organization to Office 365

You must define a target address for your mailboxes that are hosted in Exchange Online. This target address is created automatically when your Office 365 organization is created. For example, if your organization's domain hosted in the Office 365 organization is "contoso.com", your target service address would be "contoso.mail.onmicrosoft.com".

Using Exchange PowerShell, run the following cmdlet in your on-premises organization:

```powershell
$ServiceDomain = Get-AcceptedDomain | where {$_.DomainName -like "*.mail.onmicrosoft.com"} | select -ExpandProperty Name
New-IntraOrganizationConnector -name ExchangeHybridOnPremisesToOnline -DiscoveryEndpoint https://outlook.office365.com/autodiscover/autodiscover.svc -TargetAddressDomains $ServiceDomain
```

### Step 7: Create an IntraOrganizationConnector from your Office 365 organization to your on-premises Exchange organization

You must define a target address for your mailboxes that are hosted in your on-premises organization. If you organization's primary SMTP adddress is "contoso.com", this would be "contoso.com".
You must also define the external Autodiscover endpoint for your on-premises organization. If your company is "contoso.com" this is usually either of the following:

- https://autodiscover.\<your primary SMTP domain\>/autodiscover/autodiscover.svc

- https://\<your primary SMTP domain\>/autodiscover/autodiscover.svc

> [!NOTE]
> You can use the [Get-IntraOrganizationConfiguration](https://docs.microsoft.com/powershell/module/exchange/Get-IntraOrganizationConfiguration) cmdlet in both your on-premises and Office 365 tenants to determine the endpoint values needed by [New-IntraOrganizationConnector](https://docs.microsoft.com/powershell/module/exchange/New-IntraOrganizationConnector) cmdlet.

Using Windows PowerShell, run the following cmdlet:

```powershell
$UserCredential = Get-Credential
$Session = New-PSSession -ConfigurationName Microsoft.Exchange -ConnectionUri https://outlook.office365.com/powershell-liveid/ -Credential $UserCredential -Authentication Basic -AllowRedirection
Import-PSSession $Session
New-IntraOrganizationConnector -name ExchangeHybridOnlineToOnPremises -DiscoveryEndpoint <your on-premises Autodiscover endpoint> -TargetAddressDomains <your on-premises SMTP domain>
```

### Step 8: Configure an AvailabilityAddressSpace for any pre-Exchange 2013 SP1 servers

When you configure a hybrid deployment in a pre-Exchange 2013 organization, you have to install at least one Exchange 2013 SP1 or greater server with the Client Access and Mailbox server roles in your existing Exchange organization. The Exchange 2013 Client Access and Mailbox servers serve as frontend servers and coordinate communications between your existing Exchange on-premises organization and the Exchange Online organization. This communication includes message transport and messaging features between the on-premises and Exchange Online organizations. We highly recommend installing more than one Exchange 2013 server in your on-premises organization to help increase reliability and availability of hybrid deployment features.

In a mixed deployment with Exchange 2013/2010 or Exchange 2013/2007, it is recommended that all the Internet-facing frontend servers for your on-premises organization are Client Access servers running Exchange 2013 SP1 or greater. All Exchange Web Services (EWS) requests originating from Office 365 and Exchange Online must connect to an Exchange 2013 Client Access server(s) in your on-premises deployment. Additionally, all EWS requests originating in your on-premises Exchange organizations for Exchange Online must be proxied through a Client Access server running Exchange 2013 SP1 or greater. Since these Exchange 2013 Client Access servers have to handle this additional incoming and outgoing EWS requests, it is important to have a sufficient number of Exchange 2013 Client Access servers available to handle the processing load and provide connection redundancy. The number of Client Access servers needed will depend on the average amount of EWS requests and will vary by organization.

Before you complete the following step, make sure:

- The frontend hybrid servers are Exchange 2013 SP1 or greater

- You have a unique external EWS URL for the Exchange 2013 server(s). The Office 365 organization must connect to these servers in order for cloud-based requests for hybrid features to work correctly.

- The servers have both the Mailbox and Client Access server roles

- Any existing Exchange 2010/2007 Mailbox and Client Access servers have the latest Cumulative Update (CU) or Service Pack (SP) applied.

> [!NOTE]
> Existing Exchange 2010/2007 Mailbox servers can continue to use Exchange 2010/2007 Client Access servers for frontend servers for non-hybrid feature connections. Only hybrid deployment feature requests from the Office 365 organization need to connect to Exchange 2013 servers.

An *AvailabilityAddressSpace* must be configured on pre-Exchange 2013 Client Access servers that points to the Exchange Web Services endpoint of your on-premises Exchange 2013 SP1 Client Access server(s). This endpoint is the same endpoint as previously outlined in Step 5 or can be determined by running the following cmdlet on your on-premises Exchange 2013 SP1 Client Access server:

```powershell
Get-WebServicesVirtualDirectory | Format-List AdminDisplayVersion,ExternalUrl
```

> [!NOTE]
> If virtual directory information is returned from multiple servers, make sure you use the endpoint returned for an Exchange 2013 SP1 Client Access server. It will display 15.0 (Build 847.32) or higher for the <EM>AdminDisplayVersion</EM> parameter.

To configure the *AvailabilityAddressSpace*, use Exchange PowerShell and run the following cmdlet in your on-premises organization:

```powershell
Add-AvailabilityAddressSpace -AccessMethod InternalProxy -ProxyUrl <your on-premises External Web Services URL> -ForestName <your Office 365 service target address> -UseServiceAccount $True
```

## How do you know this worked?

You can verify that the OAuth configuration is correct by using the [Test-OAuthConnectivity](https://docs.microsoft.com/powershell/module/exchange/Test-OAuthConnectivity) cmdlet. This cmdlet verifies that the on-premises Exchange and Exchange Online endpoints can successful authenticate requests from each other.

> [!IMPORTANT]
> When connecting to your Exchange Online organization using Remote PowerShell, you may have to use the <EM>AllowClobber</EM> parameter with the <STRONG>Import-PSSession</STRONG> cmdlet to import the latest commands in to the local PowerShell session.

To verify that your on-premises Exchange organization can successfully connect to Exchange Online, run the following command in Exchange PowerShell in your on-premises organization:

```powershell
Test-OAuthConnectivity -Service EWS -TargetUri https://outlook.office365.com/ews/exchange.asmx -Mailbox <On-Premises Mailbox> -Verbose | Format-List
```

To verify that your Exchange Online organization can successfully connect to your on-premises Exchange organization, use the [Remote PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell) to connect to your Exchange Online organization and run the following command:

```powershell
Test-OAuthConnectivity -Service EWS -TargetUri <external hostname authority of your Exchange On-Premises deployment>/metadata/json/1 -Mailbox <Exchange Online Mailbox> -Verbose | Format-List
```

So, as an example, Test-OAuthConnectivity -Service EWS -TargetUri https://mail.contoso.com/metadata/json/1 -Mailbox ExchangeOnlineBox1 -Verbose | Format-List

> [!IMPORTANT]
> You can ignore the "The SMTP address has no mailbox associated with it." error. It's only important that the <EM>ResultTask</EM> parameter returns a value of <STRONG>Success</STRONG>. For example, the last section of the test output should read:<BR><CODE>ResultType: Success</CODE><BR><CODE>Identity: Microsoft.Exchange.Security.OAuth.ValidationResultNodeId</CODE><BR><CODE>IsValid: True</CODE><BR><CODE>ObjectState: New</CODE>

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).
