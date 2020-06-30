---
title: "Create a hybrid deployment with the Hybrid Configuration wizard"
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
f1.keywords:
- NOCSH
audience: ITPro
ms.topic: article
ms.prod: exchange-server-it-pro
localization_priority: Normal
ms.collection:
- Ent_O365_Hybrid
- Strat_EX_EXOBlocker
- Hybrid
- M365-email-calendar
ms.assetid: 997a25d3-7fb3-4d4e-bb28-defcbf542c99
ms.reviewer:
description: "By establishing a hybrid deployment, you can extend the feature-rich experience and administrative control you have with your existing on-premises Exchange Server organization to the cloud. A hybrid deployment also offers support for a cloud-based archiving solution for your on-premises mailboxes with Exchange Online Archiving and may also serve as an intermediate step towards a complete migration of your on-premises mailboxes to Exchange Online."
---

# Create a hybrid deployment with the Hybrid Configuration wizard

By establishing a hybrid deployment, you can extend the feature-rich experience and administrative control you have with your existing on-premises Exchange Server organization to the cloud. A hybrid deployment also offers support for a cloud-based archiving solution for your on-premises mailboxes with Exchange Online Archiving and may also serve as an intermediate step towards a complete migration of your on-premises mailboxes to Exchange Online.

This topic covers configuring a hybrid deployment for your on-premises Exchange organization and your Exchange Online organization in Microsoft 365 or Office 365 using the Hybrid Configuration wizard. In this topic, a hybrid deployment is created for the following organization configuration:

- The on-premises organization is a single-forest on-premises Exchange organization.

- The on-premises organization doesn't use an existing Microsoft Exchange Online Protection (EOP) service for on-premises protection.

- The on-premises organization doesn't have Edge Transport servers deployed. The Hybrid Configuration wizard supports configuring Edge Transport servers as part of a hybrid deployment, but configuring Edge Transport servers in the wizard isn't covered in this topic.

> [!IMPORTANT]
> Configuring a hybrid deployment with the Hybrid Configuration wizard requires several important prerequisites for the wizard to complete successfully and for the hybrid deployment features to function correctly. You must complete all the prerequisites outlined in [Hybrid deployment prerequisites](../hybrid-deployment-prerequisites.md) before you use the Hybrid Configuration wizard to create and configure your hybrid deployment. > Additionally, the [Exchange Server Deployment Assistant](https://technet.microsoft.com/exdeploy2013) is a free web-based tool that helps you configure a hybrid deployment between your on-premises organization and Microsoft 365 or Office 365, or to migrate completely to Microsoft 365 or Office 365. The tool asks you a small set of simple questions and then, based on your answers, creates a customized checklist with instructions to configure your hybrid deployment. We strongly recommend that you use the Deployment Assistant to generate a customized hybrid deployment checklist for your specific organization's needs.

For additional management tasks related to hybrid deployments, see [Hybrid Deployment procedures](hybrid-deployment.md).

Learn more about hybrid deployments at [Exchange Server Hybrid Deployments](../exchange-hybrid.md). Learn more about Microsoft 365 and Office 365 at [What are Microsoft 365 and Office 365?](https://www.microsoft.com/microsoft-365).

## What do you need to know before you begin?

- Estimated time to complete: 30 minutes

    > [!IMPORTANT]
    > Configuring the requirements for a hybrid deployment will take considerably longer than the estimated time to complete the Hybrid Configuration wizard procedures outlined in this topic. For example, signing up for Office 365 for enterprises, configuring Active Directory synchronization, and assigning Exchange Online licenses require a larger time investment and may also include network topology changes. You should plan for more than the time listed to complete this procedure for the overall time to complete the end-to-end hybrid deployment configuration.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Hybrid deployments" entry in the [Exchange and PowerShell infrastructure permissions](https://docs.microsoft.com/Exchange/permissions/feature-permissions/infrastructure-permissions) topic.

- You need to run the Hybrid Configuration Wizard from a computer running the latest release of a supported version of Exchange. The final steps in the Hybrid Configuration wizard for configuring Exchange OAuth authentication require that the steps are performed from an on-premises Exchange or from any domain-joined server or workstation. Additionally, the OAuth authentication process works best when using the desktop version of Internet Explorer 11 or greater.

- Review [Exchange Server Hybrid Deployments](../exchange-hybrid.md), and make sure you understand the areas that will be affected by configuring a hybrid deployment.

- Review and complete all hybrid deployment requirements outlined in [Hybrid deployment prerequisites](../hybrid-deployment-prerequisites.md).

- The Microsoft Remote Connectivity Analyzer tool checks the external connectivity of your on-premises Exchange organization and makes sure that you're ready to configure your hybrid deployment. We strongly recommend that you check your on-premises organization with the Remote Connectivity Analyzer tool prior to configuring your hybrid deployment with the Hybrid Configuration wizard. Learn more at [Remote Connectivity Analyzer](https://testconnectivity.microsoft.com/tests/o365).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](https://docs.microsoft.com/Exchange/accessibility/keyboard-shortcuts-in-admin-center).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612),[Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the Exchange admin center and Hybrid Configuration wizard to create a hybrid deployment

Use the following procedure to create and configure a hybrid deployment:

1. In the EAC on an Exchange server in your on-premises organization, navigate to the **Hybrid** node.

2. In the **Hybrid** node, click **Configure** to enter your Microsoft 365 or Office 365 credentials.

    > [!IMPORTANT]
    > If your on-premises organization is located in China and your Microsoft 365 or Office 365 organization is hosted by 21Vianet, you must select the **Office 365 organization is hosted by 21Vianet** check box. If your Office 365 tenant is hosted by 21Vianet and this checkbox isn't selected, the Hybrid Configuration wizard won't connect to 21Vianet service, your Office 365 account credentials won't be recognized and the wizard won't complete properly.

3. At the prompt to log in to Microsoft 365 or Office 365, select **sign in to Office 365** and enter the account credentials. The account you log into needs to be a Global Administrator in Microsoft 365 or Office 365.

4. Click **Configure** again to start the Hybrid Configuration wizard.

5. On the **Microsoft Office 365 Hybrid Configuration Wizard Download** page, click **Click here** to download wizard. When you're prompted, click **Install** on the **Application Install** dialog.

6. Click **Next**, and then, in the **On-premises Exchange Server Organization** section, select **Detect a server running Exchange 2013 CAS or Exchange 2016**. The wizard will attempt to detect an on-premises Exchange server. If the wizard doesn't detect an Exchange server, or if you want to use a different server, select **Specify a server running Exchange 2013 CAS or Exchange 2016** and then specify the internal FQDN of an Exchange Mailbox server.

7. In the **Office 365 Exchange Online** section, select **Microsoft Office 365** and then click **Next**.

8. On the **Credentials** page, in the **Enter your on-premises account credentials** section, select **Use current Windows credentials** to have the wizard use the account you're logged into to access your on-premises Active Directory and Exchange servers. If you want to specify a different set of credentials, unselect **Use current Windows credentials** and specify the username and password an Active Directory account you want to use. Whichever selection you choose, the account used needs to be a member of the Enterprise Admins security group.

9. In the **Enter your Office 365** credentials section, specify the username and password of a Microsoft 365 or Office 365 account that has Global Administrator permissions. Click **Next**.

10. On the **Validating Connections and Credentials** page, the wizard will connect to both your on-premises organization and your Microsoft 365 or Office 365 organization to validate credentials and examine the current configuration of both organizations. Click **Next** when it's done.

11. On the **Hybrid Domains**, select the domains you want to include in your hybrid deployment. In most deployments you can leave the **Auto Discover** column set to **False** for each domain. Only select **True** next to a domain if you need to force the wizard to use the Autodiscover information from a specific domain. Click **Next**.

    > [!IMPORTANT]
    > This domain selection step of the Hybrid Configuration wizard may or may not appear when you run the wizard. This step won't appear if: <br/>• You have only one on-premises accepted domain added to your Office 365 organization. Because this is the only domain available for hybrid deployment configuration, the domain is automatically selected and the step is skipped in the wizard. <br/>• There aren't any on-premises accepted domains added to your Office 365 organization. In this case, you'll receive an error and you'll need to add at least one domain to your Office 365 organization before continuing. You can do this by using the Office 365 Administrative portal, or by optionally configuring Active Directory Federation Services (AD FS) in your on-premises organization. <br/><br/> This step will appear if you have more than one on-premises accepted domain added to your Office 365 organization.

12. On the **Federation Trust** page, click **Enable** and click then **Next**.

13. On the **Domain Ownership** page, click **Click copy to clipboard** to copy the domain proof token information for the domains you've selected to include in the hybrid deployment. Open a text editor such as Notepad and paste the token information for these domains. Before continuing in the Hybrid Configuration wizard, you must use this info to create a TXT record for each domain in your public DNS. Refer to your DNS host's Help for information about how to add a TXT record to your DNS zone. Click **Next** after the TXT records have been created and the DNS records have replicated.

14. On the **Transport Certificate** page, in the **Select a reference server** field, select the Exchange server that has the certificate you configured earlier in the checklist.

15. In the **Select a certificate** field, select the certificate to use for secure mail transport. This list displays the digital certificates issued by a third-party certificate authority (CA) installed on the Mailbox server selected in the previous step. Click **Next**.

16. On the **Organization FQDN** page, enter the externally accessible FQDN for your Internet-facing Exchange server. Microsoft 365 and Office 365 use this FQDN to configure the service connectors for secure mail transport between your Exchange organizations. For example, enter "mail.contoso.com". Click **Next**.

17. The hybrid deployment configuration selections have been updated, and you're ready to start the Exchange services changes and the hybrid deployment configuration. Click **Update** to start the configuration process. While the hybrid configuration process is running, the wizard displays the feature and service areas that are being configured for the hybrid deployment as they are updated.

18. The wizard displays a completion message and the **Close** button is displayed. Click **Close** to complete the hybrid deployment configuration process and to close the wizard.

## Configure OAuth authentication between Exchange and Exchange Online organizations

For mixed Exchange 2013/2010 and Exchange 2013/2007 hybrid deployments, the new hybrid deployment OAuth-based authentication connection between Microsoft 365 or Office 365 and on-premises Exchange organizations isn't configured by the Hybrid Configuration wizard. These deployments continue to use the federation trust process by default. However, certain Exchange 2013 features such as Message Records Management (MRM), Exchange In-place Archiving, and In-place eDiscovery are only fully available across your organization by using the new Exchange OAuth authentication protocol. We recommend that all mixed Exchange 2013/2010 and Exchange 2013/2007 organizations that wish to implement these features as part of a new hybrid deployment with Exchange Online configure Exchange OAuth authentication after configuring their hybrid deployment with the Hybrid Configuration Wizard.

For detailed configuration steps, see [Configure OAuth Authentication Between Exchange and Exchange Online Organizations](https://docs.microsoft.com/exchange/configure-oauth-authentication-between-exchange-and-exchange-online-organizations-exchange-2013-help)

For more information about Exchange security and compliance features that use OAuth authentication, see:

- [Using OAuth authentication to support Archiving in an Exchange hybrid deployment](https://docs.microsoft.com/exchange/using-oauth-authentication-to-support-archiving-in-an-exchange-hybrid-deployment-exchange-2013-help)

- [Using Oauth Authentication to Support eDiscovery in an Exchange Hybrid Deployment](https://docs.microsoft.com/exchange/using-oauth-authentication-to-support-ediscovery-in-an-exchange-hybrid-deployment-exchange-2013-help)

## How do you know this worked?

The successful completion of the Hybrid Configuration wizard will be your first indication the completion of the hybrid configuration steps worked as expected.

To further verify that you have successfully created and configured your hybrid deployment, do the following:

- Run the following command in the Exchange Management Shell for the on-premises organization. This command displays the hybrid deployment configuration values and settings, hybrid features, and transport endpoints. Verify that these values are correct.

  ```PowerShell
  Get-HybridConfiguration
  ```

- Confirm that the Hybrid Configuration wizard completed all the configuration steps by examining the hybrid configuration log. By default, the log is located at C:\Program Files\Microsoft\Exchange Server\V15\Logging\Update-HybridConfiguration on the on-premises Mailbox server.

- Move an existing on-premises mailbox to the Exchange Online organization to test the mailbox move feature support, or create a new user mailbox in the Exchange Online organization to test free/busy calendar sharing between the two organizations. Either mailbox action will also allow you to test and confirm that message delivery between the on-premises and Exchange Online organizations is functioning correctly with existing mailboxes and that message delivery is secure and treated as internal messages to the Exchange organization.

  - Use the EAC and navigate to **Enterprise** \> **Recipients** \> **Mailboxes** to create a new remote mailbox in Exchange Online.

  - Use the EAC and navigate to **Office 365** \> **Recipients** \> **Migration** to move an existing mailbox to Exchange Online.
