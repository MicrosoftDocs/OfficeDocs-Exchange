---
localization_priority: Normal
description: Admins can learn how to integrate Office 365 with a third-party service that affects mail flow.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 7c5b7408-bfa2-4695-a6b7-21ddc8084d52
ms.reviewer: 
f1.keywords:
- NOCSH
title: Scenario Integrate Office 365 with an email add-on service
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Scenario: Integrate Office 365 with an email add-on service

Many third-party cloud service solutions provide add-on services for Office 365. For security reasons, we don't allow third-party email add-on services to be installed in Office 365. But, you can work with the service provider to configure the settings in your Microsoft 365 or Office 365 organization so you can use the service.

This topic describes the best practices for how your organization can use a third-party email add-on service by examining a fictional service named Contoso Signature Service. This fictional service runs in Azure and provides custom email signatures (note that the service could be deployed in a cloud environment other than Azure). The mail flow and a high-level summary of the service are shown in the following diagram.

![Functional diagram for fictional Contoso Signature Service email add-on service](../../media/a38e832b-0b37-49d8-9074-c9dc6f7710b3.png)

1. When a user in your Microsoft 365 or Office 365 organization composes and sends a message, the message is diverted to Contoso Signature Service by using a connector and a mail flow rule (also known as a transport rule) that you create.

   Connections from Office 365 to Contoso Signature Service are encrypted by TLS, because you configure the certificate domain name for the service in the connector settings (for example, smtp.contososignatureservice.com).

2. Contoso Signature Service accepts the message and adds an email signature to the message. The service also stamps the message with a custom header to indicate the message has been processed.

3. Contoso Signature Service routes the message back to Office 365. A connector that you create accepts the incoming messages from Contoso Signature Service.

   - Contoso Signature Service uses smart host routing to route messages back to the region where your Microsoft 365 or Office 365 organization is located. For example, if your Office 365 domain is fabrikam.onmicrosoft.com, the destination smart host is fabrikam.mail.protection.outlook.com.

   - Contoso Signature Service provides a unique certificate domain name for each customer. You configure this domain name as an accepted domain in your Microsoft 365 or Office 365 organization, and in the connector settings (for example, S5HG3DCG14H8S1R2303RZHM4RX.smtp.contososignatureservice.com).

4. Office 365 sends the message with the customized signature to the original recipients.

The rest of this topic explains how to configure mail flow in Office 365 to work with the email add-on service.

> [!NOTE]
> These elements are required for any email add-on service that you want to integrate with your Microsoft 365 or Office 365 organization. You need to work with the email add-on service provider to configure their required settings in Office 365.

## What do you need to know before you begin?

- Estimated time to complete: 15 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mail flow" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- To open the Exchange admin center (EAC), see [Exchange admin center in Exchange Online](../../exchange-admin-center.md). To learn how to use Windows PowerShell to connect to Exchange Online, see [Connect to Exchange Online PowerShell](https://go.microsoft.com/fwlink/p/?linkid=396554).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Step 1: Create an outbound connector to route messages to the email add-on service

The important settings for the connector are:

- From Office 365 to the email add-on service.

- Uses smart host routing to the email add-on service.

- Uses TLS to encrypt the connection based on the domain name of the email add-on service (smart host).

### Use the EAC to create the outbound connector to the email add-on service

1. In the EAC, go to **Mail flow** \> **Connectors**, and then click **New** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif).

   ![In the Exchange admin center, click Mal flow \> Connectors to add a new connector.](../../media/6806c52b-5e5d-447c-91f7-c5fa4cd8b19d.png)

2. The new connector wizard opens. On the **Select your mail flow scenario** page, configure these settings:

   - **From**: **Office 365**

   - **To**: **Your organization's email server**

   ![In the new connector wizard, select From \> Office 365 and To \> Your organization's email server.](../../media/852fbdf3-6829-413a-9d08-63421efd10c6.png)

   When you're finished, click **Next**.

3. On the next page, configure these settings:

   - **Name**: Enter a descriptive name (for example, Office 365 to Contoso Signature Service).

   - **Retain internal Exchange email headers (recommended)**: Configure one of these values:

   - **Checked**: Preserves internal headers in messages that are sent to the email add-on service, which means the messages are treated as trusted internal messages. If you select this value, you'll also need to use the same value on this setting for the inbound connector that you create in Step 4 (otherwise, the inbound connector will remove the internal Exchange headers from the returning messages).

   - **Unchecked**: Removes internal headers from messages before they're sent to the email add-on service. If you select this value, the value of this setting on the inbound connector that you create in Step 4 is meaningless (by definition, there will be no internal Exchange headers to keep or remove in returning messages).

   ![In the new connector wizard, enter a descriptive name for the connector.](../../media/772699f4-1687-48d6-9482-72f2cc7c4ea5.png)

   When you're finished, click **Next**.

4. On the **When do you want to use this connector?** page, select **Only when I have a transport rule set up that redirects messages to this connector**, and then click **Next**.

   ![In the new connector wizard, select the option to only use the connector for messages redirected by a mail flow rule.](../../media/c02fc961-6227-4c23-ba54-9cce05e6689e.png)

5. On the **How do you want to route email messages?** page, click **Add** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif). In the **Add smart host** dialog that appears, enter the smart host value for the email add-on service (for example, smtp.contososignatureservice.com), click **Save**, and then click **Next**.

   ![In the new connector wizard, enter the smart host destination for the connector.](../../media/d24e4a9f-bab4-4300-9a8c-c17432fedb5c.png)

6. On the **How should Office 365 connect to your email server?** page, configure these settings:

   - Verify **Always use Transport Layer Security (TLS) to secure the connection (recommended)** is selected.

   - Verify **Issued by a trusted certificate authority (CA)** is selected.

   - Select **And the subject name or subject alternative name (SAN) matches this domain name**, and enter the smart host that you used in the previous step (for example, smtp.contososignatureservice.com).

   ![In the new connector wizard, use TLS and identify the certificate domain name for connections to Office 365](../../media/2ce67ff7-c6d1-49ae-9790-28c27396ab18.png)

   When you're finished, click **Next**.

7. On the **Confirm your settings** page, verify the settings, and then click **Next**.

   ![In the new connector wizard, verify the settings.](../../media/58bf30f3-456b-4671-a9bf-cca682d7dfda.png)

8. On the **Validate this connector** page, click **Add** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif). In the **Add email** dialog that appears, enter an email address that isn't in Office 365 to test the connector (for example, admin@fabrikam.com), click **OK**, and then click **Validate**.

   ![In the new connector wizard, enter an email address to validate the connector.](../../media/8bf14376-b2dd-4b7c-a379-4274fd042dae.png)

   A progress indicator appears. When the connector validation is complete, click **Close**.

   ![The connector validation progress indicator in the new connector wizard.](../../media/35681101-5c07-4c25-aef2-4e06cb423425.png)

9. On the **Validation result** page, click **Save**.

### Use Exchange Online PowerShell to create the outbound connector to the email add-on service

To create the outbound connector to the email add-on service in Exchange Online PowerShell, use this syntax:

```powershell
New-OutboundConnector -Name "<Descriptive Name>" -ConnectorType OnPremises -IsTransportRuleScoped $true -UseMxRecord $false -SmartHosts <SmartHost> -TlsSettings DomainValidation -TlsDomain <SmartHost> [-CloudServicesMailEnabled $true]
```

This example creates an outbound connector with these settings:

- **Name**: Office 365 to Contoso Signature Service

- **Smart host destination of the email add-on service**: smtp.contososignatureservice.com

- **TLS domain for domain validation**: smtp.contososignatureservice.com

- Internal Exchange message headers that identify messages as internal are preserved in the outbound messages.

```powershell
New-OutboundConnector -Name "Office 365 to Contoso Signature Service" -ConnectorType OnPremises -IsTransportRuleScoped $true -UseMxRecord $false -SmartHosts smtp.contososignatureservice.com -TlsSettings DomainValidation -TlsDomain smtp.contososignatureservice.com -CloudServicesMailEnabled $true
```

For detailed syntax and parameter information, see [New-OutboundConnector](https://docs.microsoft.com/powershell/module/exchange/new-outboundconnector).

### How do you know this step worked?

To verify that you've successfully created an outbound connector to route messages to the email add-on service, use either of these procedures:

- In the EAC, go to **Mail flow** \> **Connectors**, select the connector, click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif), and verify the settings.

- In Exchange Online PowerShell, replace _\<Connector Name\>_ with the name of the connector, and run this command to verify the property values:

  ```powershell
  Get-OutboundConnector -Identity "<Connector Name>" | Format-List Name,ConnectorType,IsTransportRuleScoped,UseMxRecord,SmartHosts,TlsSettings,TlsDomain,CloudServicesMailEnabled
  ```

## Step 2: Create a mail flow rule to route unprocessed messages to the email add-on service

The rule routes messages from internal senders to the outbound connector that you created in Step 1 if the messages haven't already been processed by the email add-on service (the custom header isn't stamped on the message).

### Use the EAC to create a mail flow rule to route unprocessed messages to the email add-on service

1. In the EAC, go to **Mail flow** \> **Rules**, and click **New** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif), and then select **Create a new rule**.

   ![In the Exchange admin center, click Mal flow \> Rules to add a new rule](../../media/568bbbf2-e69a-4d59-b7d4-b1af06655433.png)

2. In the **New rule** page that opens, click **More options** near the bottom of the page.

   ![On the new rule page, click More options.](../../media/18f546af-b5ab-4bce-ac9c-23609816da7e.png)

3. On the **New rule** page, configure these settings:

   - **Name**: Enter a descriptive name (for example, Route email to Contoso Signature Service).

   - **Apply this rule if**: Select **The sender** \> **Is external/internal** \> Select **Inside the organization**, and then click **OK**.

   - **Do the following**: Select **Redirect the message to** \> **The following connector** \> Select the outbound connector you created in Step 1, and then click **OK**.

   - **Except if**: Click **Add exception** \> Select **A message header** \> **Includes and of these words**.

   - Click **Enter text**, enter the name of the custom header field that's applied by the email add-on service (for example, SignatureContoso), and then click **OK**.

   - Click **Enter words**, enter the header field value that indicates a message has been processed by the email add-on service (for example, true), click **Add** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif), and then click **OK**.

   - Near the bottom of the page, select **Stop processing more rules**.

   ![On the new rule page, configure the settings to route messages to the email add-on service.](../../media/97e895ea-fbf5-437b-a23c-13adf8db93ac.png)

When you're finished, click **Save**.

### Use Exchange Online PowerShell to create a mail flow rule to route unprocessed messages to the email add-on service

To create the mail flow rule in Exchange Online PowerShell, use this syntax:

```powershell
New-TransportRule -Name "<Descriptive Name>" -FromScope InOrganization -RouteMessageOutboundConnector "<Connector Name>" -ExceptIfHeaderContainsMessageHeader <HeaderName> -ExceptIfHeaderContainsWords <HeaderValue> -StopRuleProcessing $true
```

This example creates the mail flow rule with these settings:

- **Name**: Route email to Contoso Signature Service

- **Outbound connector name**: Office 365 to Contoso Signature Service

- **Header field and value that indicates processing by the email add-on service**SignatureContoso with the value true.

```powershell
New-TransportRule -Name "Route email to Contoso Signature Service" -FromScope InOrganization -RouteMessageOutboundConnector "Office 365 to Contoso Signature Service" -ExceptIfHeaderContainsMessageHeader SignatureContoso -ExceptIfHeaderContainsWords true -StopRuleProcessing $true
```

For detailed syntax and parameter information, see [New-TransportRule](https://docs.microsoft.com/powershell/module/exchange/new-transportrule).

### How do you know this step worked?

To verify that you've successfully created a mail flow rule to route unprocessed messages to the email add-on service, use either of these procedures:

- In the EAC, go to **Mail flow** \> **Rules**, select the rule, click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif), and verify the settings of the rule.

- In Exchange Online PowerShell, replace _\<Rule Name\>_ with the name of the rule, and run this command to verify the property values:

  ```powershell
  Get-TransportRule -Identity "<Rule Name>" | Format-List Name,FromScope,RouteMessageOutboundConnector,ExceptIfHeaderContainsMessageHeader,ExceptIfHeaderContainsWords,StopRuleProcessing
  ```

## Step 3: Add the custom certificate domain provided by the email add-on service as an accepted domain in Office 365

1. Open the [Microsoft 365 admin center](https://admin.microsoft.com), click **Setup** \> **Domains**, and then click **Add domain**.

   ![In the admin center, go to Setup \> Domains to add the certificate domain as an accepted domain.](../../media/5d4db406-5970-45f4-8955-2bc90d087b6f.png)

2. In the **Add a domain** page that appears, enter the custom certificate domain that the email add-on service provided when you enrolled in the service (for example, S5HG3DCG14H8S1R2303RZHM4RX.smtp.contososignatureservice.com), and then click **Next**.

   ![Enter the custom certificate domain provided by the email add-on service.](../../media/b131c36a-754e-4096-8532-1c2c716ef970.png)

3. On the **Verify domain** page, use the details on the **TXT record** or **MX record** tabs to create a TXT or MX proof of domain ownership record for the custom certificate domain. After you've created the proof of domain ownership record, click **Verify**. After the domain has been verified, click **Save and close**.

   ![The TXT proof of domain ownership record for the custom certificate domain.](../../media/fd0ab875-925d-46ea-96fd-00924e6f298f.png)

For more information, see [Add your domain to Office 365](https://docs.microsoft.com/office365/admin/setup/add-domain)

## Step 4: Create an inbound connector to receive messages from the email add-on service

The important settings for the connector are:

- From the email add-on service to Office 365.

- TLS encryption and certificate verification is based on the custom certificate domain name that you configured as an accepted domain in the previous step.

### Use the EAC to create an inbound connector to receive messages from the email add-on service

1. In the EAC, go to **Mail flow** \> **Connectors**, and then click **New** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif).

   ![In the Exchange admin center, click Mal flow \> Connectors to add a new connector.](../../media/6806c52b-5e5d-447c-91f7-c5fa4cd8b19d.png)

2. The new connector wizard opens. On the **Select your mail flow scenario** page, configure these settings:

   - **From** **Your organization's email server**

   - **To** **Office 365**

   ![In the new connector wizard, select From \> Your organization's email server and From \> Office 365.](../../media/ec97de50-53a0-4392-9096-15a5c15fa238.png)

   When you're finished, click **Next**.

3. On the next page, configure these settings:

   - **Name**: Enter a descriptive name (for example, Contoso Signature Service to Office 365).

   - **Retain internal Exchange email headers (recommended)**: Configure one of these values:

   - **Checked**: Preserves internal headers in messages that are returning from the email add-on service. If you selected this value on this setting for the outbound connector that you create in Step 1, you'll need to configure the same value here. The internal Exchange headers in the returning messages are preserved, which means the messages returning from the email add-on service are treated as trusted internal messages.

   - **Unchecked**: Removes the internal Exchange headers (if any) from messages that are returning from the email add-on service.

   ![In the new connector wizard, enter a descriptive name for the connector.](../../media/945fa373-d19a-43b5-9b17-4763e21b2370.png)

   When you're finished, click **Next**.

4. On the **How should Office 365 identify email from your email server?** page, verify that the first option is selected (verify by certificate), and enter the certificate domain that the email add-on service gave to you when you enrolled in the service (for example, S5HG3DCG14H8S1R2303RZHM4RX.smtp.contososignatureservice.com).

   ![In the new connector wizard, specify the unique certificate domain name that the email add-on service gave to you.](../../media/ee7a3e8f-51de-4451-8563-0f25656bddc1.png)

   When you're finished, click **Next**.

5. On the **Confirm your settings** page, verify the settings, and then click **Save**.

   ![In the new connector wizard, verify the settings and click Save.](../../media/a9b67424-159d-45b8-9296-d103ca8eac87.png)

### Use Exchange Online PowerShell to create an inbound connector to receive messages from the email add-on service

To create the inbound connector from the email add-on service in Exchange Online PowerShell, use this syntax:

```powershell
New-InboundConnector -Name "<Descriptive Name>" -SenderDomains * -ConnectorType OnPremises -RequireTls $true -RestrictDomainsToCertificate $true -TlsSenderCertificateName <CertificateDomainName> [-CloudServicesMailEnabled $true]
```

This example creates an inbound connector with these settings:

- **Name**: Contoso Signature Service to Office 365

- **Domain name used by the email add-on service's certificate to authenticate with your Microsoft 365 or Office 365 organization**: S5HG3DCG14H8S1R2303RZHM4RX.smtp.contososignatureservice.com

- Internal Exchange message headers that identify messages returning from the email add-on service as internal messages are preserved.

```powershell
New-InboundConnector -Name "Contoso Signature Service to Office 365" -SenderDomains * -ConnectorType OnPremises -RequireTls $true -RestrictDomainsToCertificate $true -TlsSenderCertificateName S5HG3DCG14H8S1R2303RZHM4RX.smtp.contososignatureservice.com -CloudServicesMailEnabled $true
```

For detailed syntax and parameter information, see [New-InboundConnector](https://docs.microsoft.com/powershell/module/exchange/new-inboundconnector).

### How do you know this step worked?

To verify that you've successfully created an inbound connector to receive messages from the email add-on service, use any of these procedures:

- In the EAC, go to **Mail flow** \> **Connectors**, select the connector, click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif), and verify the settings.

- In Exchange Online PowerShell, replace _\<Connector Name\>_ with the name of the connector, and run this command to verify the property values:

  ```powershell
  Get-InboundConnector -Identity "<Connector Name>" | Format-List Name,SenderDomains,ConnectorType,RequireTls,RestrictDomainsToCertificate,TlsSenderCertificateName,CloudServicesMailEnabled
  ```
