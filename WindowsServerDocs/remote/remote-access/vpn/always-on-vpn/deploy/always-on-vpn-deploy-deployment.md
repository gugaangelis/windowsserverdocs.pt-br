---
title: Implantar VPN Always On
description: This topic provides detailed instructions for deploying Always On VPN in Windows Server 2016.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: ad748de2-d175-47bf-b05f-707dc48692cf
ms.localizationpriority: medium
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 33e00134960ca31ce966198ded0692550e164fd6
ms.sourcegitcommit: 07c9d4ea72528401314e2789e3bc2e688fc96001
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76822399"
---
# <a name="deploy-always-on-vpn"></a>Implantar VPN Always On

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Previous:** Learn about the Always On VPN advanced features](always-on-vpn-adv-options.md)
- [**Next:** Step 1. Start planning the Always On VPN deployment](always-on-vpn-deploy-planning.md)

In this section, you learn about the workflow for deploying Always On VPN connections for remote domain-joined Windows 10 client computers. If you want to **configure conditional access** to fine-tune how VPN users access your resources, see [Conditional access for VPN connectivity using Azure AD](../../ad-ca-vpn-connectivity-windows10.md). To learn more about conditional access for VPN connectivity using Azure AD, see [Conditional access in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). 

The following diagram illustrates the workflow process for the different scenarios when deploying Always On VPN:

![Flow chart of the Always On VPN deployment workflow](../../../../media/Always-On-Vpn/always-on-vpn-deployment-workflow-sm.png)

>[!IMPORTANT]
>For this deployment, it is not a requirement that your infrastructure servers, such as computers running Active Directory Domain Services, Active Directory Certificate Services, and Network Policy Server, are running Windows Server 2016. You can use earlier versions of Windows Server, such as Windows Server 2012 R2, for the infrastructure servers and for the server that is running Remote Access.

## <a name="step-1-plan-the-always-on-vpn-deploymentalways-on-vpn-deploy-planningmd"></a>[Step 1. Plan the Always On VPN Deployment](always-on-vpn-deploy-planning.md)

In this step, you start to plan and prepare your Always On VPN deployment. Before you install the Remote Access server role on the computer you're planning on using as a VPN server. Após o planejamento adequado, você pode implantar Always On VPN e, opcionalmente, configurar o acesso condicional para conectividade VPN usando o Azure AD.

## <a name="step-2-configure-the-always-on-vpn-server-infrastructurevpn-deploy-server-infrastructuremd"></a>[Step 2. Configure the Always On VPN Server Infrastructure](vpn-deploy-server-infrastructure.md)

In this step, you install and configure the server-side components necessary to support the VPN. The server-side components include configuring PKI to distribute the certificates used by users, the VPN server, and the NPS server.  You also configure RRAS to support IKEv2 connections and the NPS server to perform authorization for the VPN connections.

To configure the server infrastructure, you must perform the following tasks:

- **On a server configured with Active Directory Domain Services:** Enable certificate autoenrollment in Group Policy for both computers and users, create the VPN Users Group, the VPN Servers Group, and the NPS Servers Group, and add members to each group.
- **Em um Active Directory AC do servidor de certificado:** Crie os modelos autenticação de usuário, autenticação de servidor VPN e certificado de autenticação de servidor NPS.
- **Em clientes do Windows 10 ingressados no domínio:** Registrar e validar certificados de usuário.

## <a name="step-3-configure-the-remote-access-server-for-always-on-vpnvpn-deploy-rasmd"></a>[Etapa 3. Configurar o servidor de acesso remoto para VPN Always On](vpn-deploy-ras.md)

Nesta etapa, você configura a VPN de acesso remoto para permitir conexões VPN IKEv2, negar conexões de outros protocolos VPN e atribuir um pool de endereços IP estáticos para a emissão de endereços IP para conectar clientes VPN autorizados.

Para configurar o RAS, você deve executar as seguintes tarefas:

- Registrar e validar o certificado do servidor VPN
- Instalar e configurar a VPN de acesso remoto

## <a name="step-4-install-and-configure-the-nps-servervpn-deploy-npsmd"></a>[Etapa 4. Instalar e configurar o servidor NPS](vpn-deploy-nps.md)

Nesta etapa, você instala o NPS (servidor de políticas de rede) usando o Windows PowerShell ou o assistente Gerenciador do Servidor adicionar funções e recursos. Você também configura o NPS para lidar com todas as tarefas de autenticação, autorização e contabilidade para a solicitação de conexão que ele recebe do servidor VPN.

Para configurar o NPS, você deve executar as seguintes tarefas:

- Registrar o servidor NPS no Active Directory
- Configurar a contabilização RADIUS para seu servidor NPS
- Adicionar o servidor VPN como um cliente RADIUS no NPS
- Configurar a política de rede no NPS
- Registrar automaticamente o certificado do servidor NPS

## <a name="step-5-configure-dns-and-firewall-settings-for-always-on-vpnvpn-deploy-dns-firewallmd"></a>[Etapa 5. Definir configurações de DNS e firewall para Always On VPN](vpn-deploy-dns-firewall.md)

Nesta etapa, você define as configurações de DNS e firewall. Quando os clientes VPN remotos se conectam, eles usam os mesmos servidores DNS que seus clientes internos usam, o que permite que eles resolvam nomes da mesma maneira que o restante das estações de trabalho internas. 

## <a name="step-6-configure-windows-10-client-always-on-vpn-connectionsvpn-deploy-client-vpn-connectionsmd"></a>[Etapa 6. Configurar conexões VPN Always On cliente do Windows 10](vpn-deploy-client-vpn-connections.md)

Nesta etapa, você configura os computadores cliente do Windows 10 para se comunicar com essa infraestrutura com uma conexão VPN. Você pode usar várias tecnologias para configurar clientes de VPN do Windows 10, incluindo o Windows PowerShell, o Microsoft Endpoint Configuration Manager e o Intune. Todos os três exigem um perfil de VPN XML para definir as configurações de VPN apropriadas.

## <a name="step-7-optional-configure-conditional-access-for-vpn-connectivityad-ca-vpn-connectivity-windows10md"></a>[Etapa 7. Adicional Configurar o acesso condicional para conectividade VPN](../../ad-ca-vpn-connectivity-windows10.md)

Nesta etapa opcional, você pode ajustar como os usuários de VPN autorizados acessam seus recursos. Com o acesso condicional do Azure AD para conectividade VPN, você pode ajudar a proteger as conexões VPN. O acesso condicional é um mecanismo de avaliação baseado em políticas que permite criar regras de acesso para qualquer aplicativo conectado do Azure AD. Para obter mais informações, consulte [acesso condicional do Azure Active Directory (AD do Azure)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).

## <a name="next-step"></a>Próximas etapas

[Etapa 1. Planeje a implantação de VPN do Always On](always-on-vpn-deploy-planning.md): antes de instalar a função de servidor de acesso remoto no computador que você está planejando usar como um servidor VPN. Após o planejamento adequado, você pode implantar Always On VPN e, opcionalmente, configurar o acesso condicional para conectividade VPN usando o Azure AD.  
