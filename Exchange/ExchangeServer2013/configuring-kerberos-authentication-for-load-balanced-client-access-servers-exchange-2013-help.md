---
title: 'Configure Kerberos authentication for load-balanced Client Access servers'
TOCTitle: Configuring Kerberos authentication for load-balanced Client Access servers
ms:assetid: 8f4faeea-a825-438d-97dc-1c398ce7aba5
ms:mtpsurl: https://technet.microsoft.com/library/Ff808312(v=EXCHG.150)
ms:contentKeyID: 62853455
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configuring Kerberos authentication for load-balanced Client Access servers

_**Applies to:** Exchange Server 2013_

**Summary:** Describes how to use Kerberos authentication with load-balanced Client Access servers in Exchange 2013.

In order for you to use Kerberos authentication with load-balanced Client Access servers, you need to complete the configuration steps described in this article.

## Create the alternate service account credential in Active Directory Domain Services

All Client Access servers that share the same namespaces and URLs need to use the same alternate service account credentials. In general, it's sufficient to have a single account for a forest for each version of Exchange. *alternate service account credential* or *ASA credential*.

> [!IMPORTANT]
> Exchange 2010 and Exchange 2013 can't share the same ASA credential. You need to create a new ASA credential for Exchange 2013.

> [!IMPORTANT]
> While CNAME records are supported for shared namespaces, Microsoft recommends using A records. This ensures that the client correctly issues a Kerberos ticket request based on the shared name, and not the server FQDN.

When you set up the ASA credential, keep these guidelines in mind:

- **Account type**: We recommend that you create a computer account instead of a user account. A computer account doesn't allow interactive logon and may have simpler security policies than a user account. If you create a computer account, the password doesn't expire, but we recommend you update the password periodically anyway. You can use local group policy to specify a maximum age for the computer account and scripts to periodically delete computer accounts that do not meet current policies. Your local security policy also determines when you need to change the password. Although we recommend you use a computer account, you can create a user account.

- **Account name**: There are no requirements for the name of the account. You can use any name that conforms to your naming scheme.

- **Account group**: The account you use for the ASA credential doesn't need special security privileges. If you are using a computer account then the account only needs to be a member of the Domain Computers security group. If you are using a user account then the account only needs to be a member of the Domain Users security group.

- **Account password**: The password you provide when you create the account will be used. So when you create the account, you should use a complex password and ensure that the password conforms to your organization's password requirements.

### To create the ASA credential as a computer account

1. On a domain-joined computer, run Windows PowerShell or the Exchange Management Shell.

    Use the **Import-Module** cmdlet to import the Active Directory module.

    ```powershell
    Import-Module ActiveDirectory
    ```

2. Use the **New-ADComputer** cmdlet to create a new Active Directory computer account using this cmdlet syntax:

    ```powershell
    New-ADComputer [-Name] <string> [-AccountPassword <SecureString>] [-AllowReversiblePasswordEncryption <System.Nullable[boolean]>] [-Description <string>] [-Enabled <System.Nullable[bool]>]
    ```

    **Example:**

    ```powershell
    New-ADComputer -Name EXCH2013ASA -AccountPassword (Read-Host 'Enter password' -AsSecureString) -Description 'Alternate Service Account credentials for Exchange' -Enabled:$True -SamAccountName EXCH2013ASA
    ```

    Where *EXCH2013ASA* is the name of the account, the description *Alternate Service Account credentials for Exchange* is whatever you want it to be, and the value for the *SamAccountName* parameter, in this case *EXCH2013ASA*, need to be unique in your directory.

3. Use the **Set-ADComputer** cmdlet to enable the AES 256 encryption cipher support used by Kerberos using this cmdlet syntax:

    ```powershell
    Set-ADComputer [-Name] <string> [-add @{<attributename>="<value>"]
    ```

    **Example:**

    ```powershell
    Set-ADComputer EXCH2013ASA -add @{"msDS-SupportedEncryptionTypes"="28"}
    ```

    Where *EXCH2013ASA* is the name of the account and the attribute to be modified is *msDS-SupportedEncryptionTypes* with a decimal value of 28, which enables the following ciphers: RC4-HMAC, AES128-CTS-HMAC-SHA1-96, AES256-CTS-HMAC-SHA1-96.

For more information about these cmdlets, see [Import-Module](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Import-Module) and [New-ADComputer](https://docs.microsoft.com/powershell/module/activedirectory/new-adcomputer).

## Cross-forest scenarios

If you have a cross-forest or resource-forest deployment, and you have users that are outside the Active Directory forest that contains Exchange, you need to configure forest trust relationships between the forests. Also, for each forest in the deployment, you need to set up a routing rule that enables trust between all name suffixes within the forest and across forests. For more information about managing cross-forest trusts, see [Configuring Partner Organizations](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/configuring-partner-organizations).

## Identify the Service Principal Names to associate with the ASA credential

After you create the ASA credential, you need to associate Exchange Service Principal Names (SPNs) with the ASA credential. The list of Exchange SPNs may vary with your configuration, but should include at least the following:

**http/**: Use this SPN for Outlook Anywhere, MAPI over HTTP, Exchange Web Services, Autodiscover, and Offline Address Book.

The SPN values need to match the service name on the network load balancer instead of on individual servers. To help plan which SPN values you should use, consider the following scenarios:

- Single Active Directory site

- Multiple Active Directory sites

In each of these scenarios, assume that the load-balanced, fully-qualified domain names (FQDNs) have been deployed for the internal URLs, external URLs, and the autodiscover internal URI used by the Client Access server members. For more information, see Understanding proxying and redirection.

## Single Active Directory site

If you have a single Active Directory site, your environment may resemble the one in the following figure:

![CAS Array with Single AD and Kerberos auth](images/Ff808312.97a1a926-f4ac-4498-bc6b-32e7fb1b70f1(EXCHG.150).jpg "CAS Array with Single AD and Kerberos auth")

Based on the FQDNs that are used by the internal Outlook clients in the preceding figure, you need to associate the following SPNs with the ASA credential:

- http/mail.corp.tailspintoys.com

- http/autodiscover.corp.tailspintoys.com

## Multiple Active Directory sites

If you have multiple Active Directory sites, your environment may resemble the one in the following figure:

![CAS array with multiple AD sites and Kerberos auth](images/Ff808312.95b52bd8-7074-4055-8bd2-e6bf1f112b42(EXCHG.150).jpg "CAS array with multiple AD sites and Kerberos auth")

Based on the FQDNs that are used by the Outlook clients in the preceding figure, you would need to associate the following SPNs with the ASA credential that is used by the Client Access servers in ADSite 1:

- http/mail.corp.tailspintoys.com

- http/autodiscover.corp.tailspintoys.com

You would also need to associate the following SPNs with the ASA credential that is used by the Client Access servers in ADSite 2:

- http/mailsdc.corp.tailspintoys.com

- http/autodiscoversdc.corp.tailspintoys.com

## Configure and then verify configuration of the ASA credential on each Client Access server

After you've created the account, you need to verify that the account has replicated to all AD DS domain controllers. Specifically, the account needs to be present on each Client Access server that will use the ASA credential. Next, you configure the account as the ASA credential on each Client Access server in your deployment.

You configure the ASA credential by using the Exchange Management Shell as described in one of these procedures:

- Deploy the ASA credential to the first Exchange 2013 Client Access server

- Deploy the ASA credential to subsequent Exchange 2013 Client Access servers

The only supported method for deploying the ASA credential is to use the RollAlternateServiceAcountPassword.ps1 script. For more information, see [Using the RollAlternateserviceAccountCredential.ps1 Script in the Shell](using-the-rollalternateserviceaccountcredential-ps1-script-in-the-shell-exchange-2013-help.md). After the script has run, we recommend that you verify that all the targeted servers have been updated correctly.

## Deploy the ASA Credential to the first Exchange 2013 Client Access server

1. Open the Exchange Management Shell on an Exchange 2013 server.

2. Change directories to *\<Exchange 2013 installation directory\>*\\V15\\Scripts.

3. Run the following command to deploy the ASA credential to the first Exchange 2013 Client Access server:

    ```powershell
    .\RollAlternateServiceAccountPassword.ps1 -ToSpecificServer cas-1.corp.tailspintoys.com -GenerateNewPasswordFor tailspin\EXCH2013ASA$
    ```

4. When you're asked if you want to change the password for the alternate service account, answer **Yes**.

The following is an example of the output that's shown when you run the RollAlternateServiceAccountPassword.ps1 script.

```powershell
========== Starting at 01/12/2015 10:17:47 ==========
Creating a new session for implicit remoting of "Get-ExchangeServer" command...
Destination servers that will be updated:

Name                                                        PSComputerName
----                                                        --------------
cas-1                                                   cas-1.corp.tailspintoys.com

Credentials that will be pushed to every server in the specified scope (recent first):

UserName
Password
--------
--------
tailspin\EXCH2013ASA$
System.Security.SecureString

Prior to pushing new credentials, all existing credentials that are invalid or no longer work will be removed from  the destination servers.
Pushing credentials to server cas-1
Setting a new password on Alternate Serice Account in Active Directory

Password change
Do you want to change password for tailspin\EXCH2013ASA$ in Active Directory at this time?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y
Preparing to update Active Directory with a new password for tailspin\EXCH2013ASA$ ...
Resetting a password in the Active Directory for tailspin\EXCH2013ASA$ ...
New password was successfully set to Active Directory.
Retrieving the current Alternate Service Account configuration from servers in scope
Alternate Service Account properties:

StructuralObjectClass QualifiedUserName Last Pwd Update       SPNs
--------------------- ----------------- ---------------       ----
computer              tailspin\EXCH2013ASA$   1/12/2015 10:19:53 AM

Per-server Alternate Service Account configuration as of the time of script completion:

   Array: {mail.corp.tailspintoys.com}

Identity  AlternateServiceAccountConfiguration
--------  ------------------------------------
cas-1 Latest: 1/12/2015 10:19:22 AM, tailspin\EXCH2013ASA$
    ...

========== Finished at 01/12/2015 10:20:00 ==========

    THE SCRIPT HAS SUCCEEDED
```

## Deploy the ASA credential to another Exchange 2013 Client Access server

1. Open the Exchange Management Shell on an Exchange 2013 server.

2. Change directories to *\<Exchange 2013 installation directory\>*\\V15\\Scripts.

3. Run the following command to deploy the ASA credential to another Exchange 2013 Client Access server:

    ```powershell
    .\RollAlternateServiceAccountPassword.ps1 -ToSpecificServer cas-2.corp.tailspintoys.com -CopyFrom cas-1.corp.tailspintoys.com
    ```

4. Repeat Step 3 for each Client Access server you want to deploy the ASA credential to.

The following is an example of the output that's shown when you run the RollAlternateServiceAccountPassword.ps1 script.

```powershell
========== Starting at 01/12/2015 10:34:35 ==========
Destination servers that will be updated:

Name                                                        PSComputerName
----                                                        --------------
cas-2                                                   cas-2.corp.tailspintoys.com

Credentials that will be pushed to every server in the specified scope (recent first):

UserName
Password
--------
--------
tailspin\EXCH2013ASA$
System.Security.SecureString

Prior to pushing new credentials, all existing credentials will be removed from the destination servers.
Pushing credentials to server cas-2
Retrieving the current Alternate Service Account configuration from servers in scope
Alternate Service Account properties:

StructuralObjectClass QualifiedUserName Last Pwd Update       SPNs
--------------------- ----------------- ---------------       ----
computer              tailspin\EXCH2013ASA$   1/12/2015 10:19:53 AM

Per-server Alternate Service Account configuration as of the time of script completion:

   Array: cas-2.corp.tailspintoys.com

Identity  AlternateServiceAccountConfiguration
--------  ------------------------------------
cas-2 Latest: 1/12/2015 10:37:59 AM, tailspin\EXCH2013ASA$
    ...

    ========== Finished at 01/12/2015 10:38:13 ==========

        THE SCRIPT HAS SUCCEEDED
```

## Verify the deployment of the ASA credential

- Open the Exchange Management Shell on an Exchange 2013 server.

- Run the following command to check the settings on a Client Access server:

  ```powershell
  Get-ClientAccessServer CAS-3 -IncludeAlternateServiceAccountCredentialStatus | Format-List Name, AlternateServiceAccountConfiguration
  ```

- Repeat Step 2 on each Client Access server where you want to verify the deployment of the ASA credential.

The following is an example of the output that's shown when you run the Get-ClientAccessServer command above and no previous ASA credential was set.

```powershell
Name                                 : CAS-1
AlternateServiceAccountConfiguration : Latest: 1/12/2015 10:19:22 AM, tailspin\EXCH2013ASA$
                                       Previous: <Not set>
                                           ...
```

The following is an example of the output that's shown when you run the Get-ClientAccessServer command above and an ASA credential was previously set. The previous ASA credential and the date and time it was set are returned.

```powershell
Name                                 : CAS-3
AlternateServiceAccountConfiguration : Latest: 1/12/2015 10:19:22 AM, tailspin\EXCH2013ASA$
                                       Previous: 7/15/2014 12:58:35 PM, tailspin\oldSharedServiceAccountName$
                                           ...
```

## Associate Service Principal Names (SPNs) with the ASA credential

> [!IMPORTANT]
> Don't associate SPNs with an ASA credential until you have deployed that credential to at least one Exchange Server, as described earlier in Deploy the ASA Credential to the first Exchange 2013 Client Access server. Otherwise, you will experience Kerberos authentication errors.

Before you associate the SPNs with the ASA credential, you need to verify that the target SPNs aren't already associated with a different account in the forest. The ASA credential need to be the only account in the forest with which these SPNs are associated. You can verify that no other account in the forest is associated with the SPNs by running the **setspn** command from the command line.

### Verify an SPN is not already associated with an account in a forest by running the setspn command

1. Press **Start**. In the **Search** box, type **Command Prompt**, then in the list of results, select **Command Prompt**.

2. At the command prompt, type the following command:

   ```powershell
   setspn -F -Q <SPN>
   ```

    Where \<SPN\> is the SPN you want to associate with the ASA credential. For example:

   ```powershell
   setspn -F -Q http/mail.corp.tailspintoys.com
   ```

   The command should return nothing. If it returns something, another account is already associated with the SPN. Repeat this step once for each SPN you want to associate with the ASA credential.

### Associate an SPN with an ASA credential by using the setspn command

1. Press **Start**. In the **Search** box, type **Command Prompt**, then in the list of results, select **Command Prompt**.

2. At the command prompt, type the following command:

    ```powershell
    setspn -S <SPN> <Account>$
    ```

    Where \<SPN\> is the SPN you want to associate with the ASA credential and \<Account\> is the account associated with the ASA credential. For example:

    ```powershell
    setspn -S http/mail.corp.tailspintoys.com tailspin\EXCH2013ASA$
    ```

    Run this command once for each SPN you want to associate with the ASA credential.

### Verify you associated the SPNs with the ASA credentials by using the setspn command

1. Press **Start**. In the **Search** box, type **Command Prompt**, then in the list of results, select **Command Prompt**.

2. At the command prompt, type the following command:

   ```powershell
   setspn -L <Account>$
   ```

   Where \<Account\> is the account associated with the ASA credential. For example:

   ```powershell
   setspn -L tailspin\EXCH2013ASA$
   ```

   You only need to run this command once.

## Enable Kerberos authentication for Outlook clients

1. Open the Exchange Management Shell on an Exchange 2013 server.

2. To enable Kerberos authentication for Outlook Anywhere clients, run the following command on your Client Access server:

   ```powershell
   Get-OutlookAnywhere -server CAS-1 | Set-OutlookAnywhere -InternalClientAuthenticationMethod  Negotiate
   ```

3. To enable Kerberos authentication for MAPI over HTTP clients, run the following on your Exchange 2013 Client Access server:

   ```powershell
   Get-MapiVirtualDirectory -Server CAS-1 | Set-MapiVirtualDirectory -IISAuthenticationMethods Ntlm, Negotiate
   ```

4. Repeat steps 2 and 3 for each Exchange 2013 Client Access server where you want to enable Kerberos authentication.

## Validate Exchange client Kerberos authentication

After you've successfully configured Kerberos and the ASA credential, verify that clients can authenticate successfully as described in these tasks.

## Verify that the Microsoft Exchange Service Host service is running

The Microsoft Exchange Service Host service (MSExchangeServiceHost) on the Client Access server is responsible for managing the ASA credential. If this service isn't running, Kerberos authentication isn't possible. By default, the service is configured to automatically start when the computer starts.

### To verify the Microsoft Exchange Service Host service is started

1. Click **Start**, type **services.msc**, and then select **services.msc** from the list.

2. In the **Services** window, locate the **Microsoft Exchange Service Host** service in the list of services.

3. The status of the service should be **Running**. If the status is not **Running**, right-click the service, and then click **Start**.

## Validate Kerberos from the Client Access server

When you configured the ASA credential on each Client Access server, you ran the **set-ClientAccessServer** cmdlet. Once you have run this cmdlet, you can use the logs to verify successful Kerberos connections.

### To validate that Kerberos is working correctly by using the HttpProxy log file

1. In a text editor, browse to the folder where the HttpProxy log is stored. By default, the log is stored in the following folder:

   ```powershell
   %ExchangeInstallPath%\\Logging\\HttpProxy\\RpcHttp
   ```

2. Open the most recent log file and look for the word **Negotiate**. The line in the log file will look something like the following example:

   ```powershell
   2014-02-19T13:30:49.219Z,e19d08f4-e04c-42da-a6be-b7484b396db0,15,0,775,22,,RpcHttp,mail.corp.tailspintoys.com,/rpc/rpcproxy.dll,,Negotiate,True,tailspin\Wendy,tailspintoys.com,MailboxGuid~ad44b1e0-e44f-4a16-9396-3a437f594f88,MSRPC,192.168.1.77,EXCH1,200,200,,RPC_OUT_DATA,Proxy,exch2.tailspintoys.com,15.00.0775.000,IntraForest,MailboxGuidWithDomain,,,,76,462,1,,1,1,,0,,0,,0,0,16272.3359,0,0,3,0,23,0,25,0,16280,1,16274,16230,16233,16234,16282,?ad44b1e0-e44f-4a16-9396-3a437f594f88@tailspintoys.com:6001,,BeginRequest=2014-02-19T13:30:32.946Z;BeginGetRequestStream=2014-02-19T13:30:32.946Z;OnRequestStreamReady=2014-02-19T13:30:32.946Z;BeginGetResponse=2014-02-19T13:30:32.946Z;OnResponseReady=2014-02-19T13:30:32.977Z;EndGetResponse=2014-02-19T13:30:32.977Z;,PossibleException=IOException;
   ```

   If you see the **AuthenticationType** value is **Negotiate**, then the server is successfully creating Kerberos authenticated connections.

## Maintain the ASA credential

If you need to refresh the password on the ASA credential periodically, use the steps for configuring the ASA credential in this article. Consider setting up a scheduled task to perform regular password maintenance. Be sure to monitor the scheduled task to ensure timely password rollovers and prevent possible authentication outages.

## Turn Kerberos authentication off

To configure your Client Access server so that it doesn't use Kerberos, disassociate or remove the SPNs from the ASA credential. If the SPNs are removed, Kerberos authentication won't be attempted by your clients, and clients configured to use Negotiate authentication will use NTLM instead. Clients configured to use only Kerberos will be unable to connect. Once the SPNs are removed you should also delete the account.

### To remove the ASA credential

1. Open the Exchange Management Shell on an Exchange 2013 server and run the following command:

   ```powershell
   Set-ClientAccessServer CAS-1 -RemoveAlternateServiceAccountCredentials
   ```

2. Although you don't have to do this immediately, you should eventually restart all client computers to clear the Kerberos ticket cache from the computer.
