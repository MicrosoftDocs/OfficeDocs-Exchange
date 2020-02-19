---
title: 'Renew the federation certificate: Exchange 2013 Help'
TOCTitle: Renew the federation certificate
ms:assetid: 0f390713-3058-44d0-9c07-3c982616e790
ms:mtpsurl: https://technet.microsoft.com/library/Mt779252(v=EXCHG.150)
ms:contentKeyID: 74429166
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Renew the federation certificate

This topic explains how to update the self-signed federation certificate that's used in a federation trust:

- If the federation certificate hasn't expired, follow the steps in the Update a working federation certificate section.

- If the federation certificate has already expired, follow the steps in the Replace an expired federation certificate section.

For more information about federation trusts and federation, see [Federation](federation-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 10 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Federation and certificates" entry in the [Exchange and Shell infrastructure permissions](exchange-and-shell-infrastructure-permissions-exchange-2013-help.md) topic.

- The procedures in this topic use the Exchange Management Shell. To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Shell](https://technet.microsoft.com/library/dd638134\(v=exchg.150\)).

- To see if your existing federation certificate has expired, run the following command in the Exchange Management Shell:

  ```powershell
  Get-ExchangeCertificate -Thumbprint (Get-FederationTrust).OrgCertificate.Thumbprint | Format-Table -Auto Thumbprint,NotAfter
  ```

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!WARNING]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Update a working federation certificate

If the federation certificate hasn't expired, you can update the existing federation trust with a new federation certificate.

## Step 1: Create a new federation certificate

Run the following command in the Exchange Management Shell to create a new federation certificate:

```powershell
$SKI = [System.Guid]::NewGuid().ToString("N"); New-ExchangeCertificate -DomainName 'Federation' -FriendlyName "Exchange Delegation Federation" -Services Federation -SubjectKeyIdentifier $SKI -PrivateKeyExportable $true
```

For detailed syntax and parameter information, see [New-ExchangeCertificate](https://technet.microsoft.com/library/aa998327\(v=exchg.150\)).

The command output contains the thumbprint value of the new certificate. You'll need this value in the remaining steps, and you can copy the value directly from the Exchange Management Shell window:

1. Right-click anywhere in the Exchange Management Shell window, and select **Mark** in the dialog that appears.

2. Select the thumbprint value, and then press ENTER.

For the other procedures in this topic, we'll use the federation certificate thumbprint value: `6A99CED2E4F2B5BE96C5D17D662D217EF58B8F73`. Your certificate thumbprint value will be different.

## Step 2: Configure the new certificate as the federation certificate

To use the Exchange Management Shell to configure the new certificate as the federation certificate, use the following syntax:

```powershell
Set-FederationTrust -Identity "Microsoft Federation Gateway" -Thumbprint <Thumbprint> -RefreshMetaData
```

This example uses the certificate thumbprint value `6A99CED2E4F2B5BE96C5D17D662D217EF58B8F73` from Step 1.

```powershell
Set-FederationTrust -Identity "Microsoft Federation Gateway" -Thumbprint 6A99CED2E4F2B5BE96C5D17D662D217EF58B8F73 -RefreshMetaData
```

For detailed syntax and parameter information, see [Set-FederationTrust](https://technet.microsoft.com/library/dd298034\(v=exchg.150\)).

**Note:** The command output contains a warning that you need to update the proof of domain ownership TXT record in DNS. You'll do that in the next step.

## Step 3: Update the federation proof of domain ownership TXT record in external DNS

You can safely perform this step now, because the proof of domain ownership TXT record is only checked during activation (Step 5). However, after you update the TXT record, and before you continue to the next step, you need to allow time for the updated TXT record to propagate (based on the time to live or TTL value of the DNS record).

1. Find the required values for the required TXT record by running the following command in the Exchange Management Shell:

   ```powershell
   Get-FederatedDomainProof -DomainName <Domain> | Format-List Thumbprint,Proof
   ```

   For example, if your federated domain is contoso.com, run the following command:

   ```powershell
   Get-FederatedDomainProof -DomainName contoso.com | Format-List Thumbprint,Proof
   ```

   The command output looks like this:

   ```console
   Thumbprint : <new certificate thumbprint> (for example, 6A99CED2E4F2B5BE96C5D17D662D217EF58B8F73)

   Proof      : <new hash text> (for example, znMfbkgSbOQSsWFdsW+gm3to0nZSdE3zbcPPHGVAqdgsLFGsCPuLHiyVbKoPmgyZKX90NH2g1PbCZH0YTQF6oA==)

   Thumbprint : <old certificate thumbprint> (for example, CC9BC204BB4DC60D06FC1F10F3C373DC785DA2A5)

   Proof      : <old hash text> (for example, m4gZX7OLr9iOWYJMVjEklQpoSkPb5hSbcFjD7Q3/vsqmdJ2Z+HcSt7j5pzBKFmEW2s27JYr3xsK2POzAI/8Ffw==)
   ```

   Note that the command output returns information for two proof of domain ownership records: one for the new certificate, and one for the current certificate that you're replacing. You can tell which is which by the thumbprint value, and the hash text value that's configured in the current proof of domain ownership TXT record in your external (public) DNS.

2. Update the federation proof of domain ownership TXT record in your external DNS. The instructions will vary based on your DNS provider, but you can edit the current TXT record to replace the current hash text value with the new hash text value. For more information, see the Exchange Online section in [External Domain Name System records for Office 365](https://go.microsoft.com/fwlink/p/?linkid=265522).

## Step 4: Verify the distribution of the new federation certificate to all Exchange servers

Exchange automatically distributes the new federation certificate to all servers, but we need to verify the distribution before we can proceed.

To use the Exchange Management Shell to verify the distribution of the new federation certificate, run the following command:

```powershell
$Servers = Get-ExchangeServer; $Servers | foreach {Get-ExchangeCertificate -Server $_ | Where {$_.Services -match 'Federation'}} | Format-List Identity,Thumbprint,Services,Subject
```

**Note:** In Exchange 2010, the output of the **Test-FederationCertificate** cmdlet contains server names. The output of the cmdlet in Exchange 2013 or later doesn't include server names.

## Step 5: Activate the new federation certificate

To use the Exchange Management Shell to activate the new federation certificate, run the following command:

```powershell
Set-FederationTrust -Identity "Microsoft Federation Gateway" -PublishFederationCertificate
```

For detailed syntax and parameter information, see [Set-FederationTrust](https://technet.microsoft.com/library/dd298034\(v=exchg.150\)).

**Note:** The command output contains a warning that you need to update the proof of domain ownership TXT record in DNS (which you already did in Step 3).

## How do you know this worked?

To verify that you've successfully updated the existing federation trust with a new federation certificate, use these steps:

- In the Exchange Management Shell, run the following command to verify that the new certificate is being used:

  ```powershell
  Get-FederationTrust | Format-List *priv*
  ```

  - The **OrgPrivCertificate** property should contain the thumbprint of the new federation certificate.

  - The **OrgPrevPrivCertificate** property should contain the thumbprint of the old (replaced) federation certificate.

- In the Exchange Management Shell, replace *\<user's email address\>* with the email address of a user in your org, and run the following command to verify that the federation trust is working:

  ```powershell
  Test-FederationTrust -UserIdentity <user's email address>
  ```

## Replace an expired federation certificate

If the federation certificate has already expired, you need to remove all federated domains from the federation trust, and then remove and recreate the federation trust.

1. If you have multiple federated domains, you need to identify the primary domain shared domain so you can remove it last. To use the Exchange Management Shell to identify the primary shared domain and all federated domains, run the following command:

   ```powershell
   Get-FederatedOrganizationIdentifier | Format-List AccountNamespace,Domains
   ```

   The value of the **AccountNamespace** property contains the primary shared domain in the format `FYDIBOHF25SPDLT<primary shared domain>`. For example, in the value `FYDIBOHF25SPDLT.contoso.com`, contoso.com is the primary shared domain.

2. Remove each federated domain that isn't the primary shared domain by running the following command in the Exchange Management Shell:

   ```powershell
   Remove-FederatedDomain -DomainName <domain> -Force
   ```

3. After you've removed all other federated domains, remove the primary shared domain by running the following command in the Exchange Management Shell:

   ```powershell
   Remove-FederatedDomain -DomainName <domain> -Force
   ```

4. Remove the federation trust by running the following command in the Exchange Management Shell:

   ```powershell
   Remove-FederationTrust "Microsoft Federation Gateway"
   ```

5. Recreate the federation trust. For instructions, see [x-Create a Federation Trust](https://technet.microsoft.com/library/dd335198\(v=exchg.150\)).
