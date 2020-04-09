---
title: Implantar VPN Always On
description: Este tópico fornece instruções detalhadas para a implantação de Always On VPN no Windows Server 2016.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: ad748de2-d175-47bf-b05f-707dc48692cf
ms.localizationpriority: medium
ms.date: 11/05/2018
ms.author: v-tea
author: Teresa-MOTIV
ms.openlocfilehash: 0889c8a3472509ec3e3a9d013ba649df7ac63d24
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860049"
---
# <a name="deploy-always-on-vpn"></a>Implantar VPN Always On

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Saiba mais sobre os recursos avançados de VPN Always On](always-on-vpn-adv-options.md)
- [**Em seguida:** Etapa 1. Começar a planejar a implantação de VPN Always On](always-on-vpn-deploy-planning.md)

Nesta seção, você aprenderá sobre o fluxo de trabalho para implantar Always On conexões VPN para computadores cliente remotos ingressados no domínio do Windows 10. Se você quiser **Configurar o acesso condicional** para ajustar como os usuários VPN acessam seus recursos, consulte [acesso condicional para conectividade VPN usando o Azure ad](../../ad-ca-vpn-connectivity-windows10.md). Para saber mais sobre o acesso condicional para conectividade VPN usando o Azure AD, consulte [acesso condicional no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). 

O diagrama a seguir ilustra o processo de fluxo de trabalho para os diferentes cenários ao implantar Always On VPN:

[![fluxograma do fluxo de trabalho de implantação de VPN Always On](../../../../media/Always-On-Vpn/always-on-vpn-deployment-workflow-sm.png)](../../../../media/Always-On-Vpn/always-on-vpn-deployment-workflow.png)

> [!IMPORTANT]
> Para essa implantação, não é um requisito de que seus servidores de infraestrutura, como computadores que executam Active Directory Domain Services, Active Directory serviços de certificados e servidor de diretivas de rede, estejam executando o Windows Server 2016. Você pode usar versões anteriores do Windows Server, como o Windows Server 2012 R2, para os servidores de infraestrutura e para o servidor que está executando o acesso remoto.

## <a name="step-1-plan-the-always-on-vpn-deployment"></a>[Etapa 1. Planejar a implantação de VPN Always On](always-on-vpn-deploy-planning.md)

Nesta etapa, você começa a planejar e preparar sua implantação de VPN Always On. Antes de instalar a função de servidor de acesso remoto no computador que você está planejando usar como um servidor VPN. Após o planejamento adequado, você pode implantar Always On VPN e, opcionalmente, configurar o acesso condicional para conectividade VPN usando o Azure AD.

## <a name="step-2-configure-the-always-on-vpn-server-infrastructure"></a>[Etapa 2. Configurar a infraestrutura de servidor VPN Always On](vpn-deploy-server-infrastructure.md)

Nesta etapa, você instala e configura os componentes do lado do servidor necessários para dar suporte à VPN. Os componentes do lado do servidor incluem a configuração de PKI para distribuir os certificados usados pelos usuários, o servidor VPN e o servidor NPS.  Você também configura o RRAS para dar suporte a conexões IKEv2 e ao servidor NPS para executar a autorização para as conexões VPN.

Para configurar a infraestrutura do servidor, você deve executar as seguintes tarefas:

- **Em um servidor configurado com Active Directory Domain Services:** Habilite o registro automático de certificado no Política de Grupo para computadores e usuários, crie o grupo de usuários VPN, o grupo servidores VPN e o grupo servidores NPS e adicione membros a cada grupo.
- **Em um Active Directory AC do servidor de certificado:** Crie os modelos autenticação de usuário, autenticação de servidor VPN e certificado de autenticação de servidor NPS.
- **Em clientes do Windows 10 ingressados no domínio:** Registrar e validar certificados de usuário.

## <a name="step-3-configure-the-remote-access-server-for-always-on-vpn"></a>[Etapa 3. Configurar o servidor de acesso remoto para VPN Always On](vpn-deploy-ras.md)

Nesta etapa, você configura a VPN de acesso remoto para permitir conexões VPN IKEv2, negar conexões de outros protocolos VPN e atribuir um pool de endereços IP estáticos para a emissão de endereços IP para conectar clientes VPN autorizados.

Para configurar o RAS, você deve executar as seguintes tarefas:

- Registrar e validar o certificado do servidor VPN
- Instalar e configurar a VPN de acesso remoto

## <a name="step-4-install-and-configure-the-nps-server"></a>[Etapa 4. Instalar e configurar o servidor NPS](vpn-deploy-nps.md)

Nesta etapa, você instala o NPS (servidor de políticas de rede) usando o Windows PowerShell ou o assistente Gerenciador do Servidor adicionar funções e recursos. Você também configura o NPS para lidar com todas as tarefas de autenticação, autorização e contabilidade para a solicitação de conexão que ele recebe do servidor VPN.

Para configurar o NPS, você deve executar as seguintes tarefas:

- Registrar o servidor NPS no Active Directory
- Configurar a contabilização RADIUS para seu servidor NPS
- Adicionar o servidor VPN como um cliente RADIUS no NPS
- Configurar a política de rede no NPS
- Registrar automaticamente o certificado do servidor NPS

## <a name="step-5-configure-dns-and-firewall-settings-for-always-on-vpn"></a>[Etapa 5. Definir configurações de DNS e firewall para Always On VPN](vpn-deploy-dns-firewall.md)

Nesta etapa, você define as configurações de DNS e firewall. Quando os clientes VPN remotos se conectam, eles usam os mesmos servidores DNS que seus clientes internos usam, o que permite que eles resolvam nomes da mesma maneira que o restante das estações de trabalho internas. 

## <a name="step-6-configure-windows-10-client-always-on-vpn-connections"></a>[Etapa 6. Configurar conexões VPN Always On cliente do Windows 10](vpn-deploy-client-vpn-connections.md)

Nesta etapa, você configura os computadores cliente do Windows 10 para se comunicar com essa infraestrutura com uma conexão VPN. Você pode usar várias tecnologias para configurar clientes de VPN do Windows 10, incluindo o Windows PowerShell, o Microsoft Endpoint Configuration Manager e o Intune. Todos os três exigem um perfil de VPN XML para definir as configurações de VPN apropriadas.

## <a name="step-7-optional-configure-conditional-access-for-vpn-connectivity"></a>[Etapa 7. Adicional Configurar o acesso condicional para conectividade VPN](../../ad-ca-vpn-connectivity-windows10.md)

Nesta etapa opcional, você pode ajustar como os usuários de VPN autorizados acessam seus recursos. Com o acesso condicional do Azure AD para conectividade VPN, você pode ajudar a proteger as conexões VPN. O acesso condicional é um mecanismo de avaliação baseado em políticas que permite criar regras de acesso para qualquer aplicativo conectado do Azure AD. Para obter mais informações, consulte [acesso condicional do Azure Active Directory (AD do Azure)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).

## <a name="next-step"></a>Próximas etapas

[Etapa 1. Planeje a implantação de VPN do Always On](always-on-vpn-deploy-planning.md): antes de instalar a função de servidor de acesso remoto no computador que você está planejando usar como um servidor VPN. Após o planejamento adequado, você pode implantar Always On VPN e, opcionalmente, configurar o acesso condicional para conectividade VPN usando o Azure AD.  
