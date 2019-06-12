---
title: Implantar VPN Always On
description: Este tópico fornece instruções detalhadas para a implantação de VPN Always On no Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: ad748de2-d175-47bf-b05f-707dc48692cf
ms.localizationpriority: medium
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6849d8547885b10fb32a133f09a82b6f133a535e
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749658"
---
# <a name="deploy-always-on-vpn"></a>Implantar VPN Always On

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Saiba mais sobre a VPN Always On recursos avançados](always-on-vpn-adv-options.md)
- [**Avançar:** Etapa 1. Comece a planejar a implantação de VPN Always On](always-on-vpn-deploy-planning.md)

Nesta seção, você aprenderá sobre o fluxo de trabalho para implantar conexões VPN Always On para computadores cliente de Windows 10 de domínio remotos. Se você quiser **configurar o acesso condicional** para ajustar como usuários da VPN acessam seus recursos, consulte [acesso condicional para conectividade VPN usando o Azure AD](../../ad-ca-vpn-connectivity-windows10.md). Para saber mais sobre o acesso condicional para conectividade VPN usando o Azure AD, consulte [acesso condicional no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). 

O diagrama a seguir ilustra o processo de fluxo de trabalho para os diferentes cenários durante a implantação de VPN Always On:

![Fluxograma de fluxo de trabalho de implantação de VPN Always On](../../../../media/Always-On-Vpn/always-on-vpn-deployment-workflow-sm.png)

>[!IMPORTANT]
>Para essa implantação, não é um requisito de que seus servidores de infraestrutura, como computadores que executam o Active Directory Domain Services, serviços de certificados do Active Directory e servidor de políticas de rede estão executando o Windows Server 2016. Você pode usar versões anteriores do Windows Server, como o Windows Server 2012 R2, para os servidores de infraestrutura e para o servidor que está executando o acesso remoto.

## <a name="step-1-plan-the-always-on-vpn-deploymentalways-on-vpn-deploy-planningmd"></a>[Etapa 1. Planejar a implantação da VPN Always On](always-on-vpn-deploy-planning.md)

Nesta etapa, você começar a planejar e preparar a implantação de VPN Always On. Antes de instalar a função de servidor de acesso remoto no computador que você estiver planejando usar como um servidor VPN. Após o planejamento adequado, você pode implantar a VPN Always On e, opcionalmente, configure o acesso condicional para conectividade VPN usando o Azure AD.

## <a name="step-2-configure-the-always-on-vpn-server-infrastructurevpn-deploy-server-infrastructuremd"></a>[Etapa 2. Configurar a infraestrutura de servidor da VPN Always On](vpn-deploy-server-infrastructure.md)

Nesta etapa, você pode instala e configurar os componentes do lado do servidor necessários para oferecer suporte a VPN. Os componentes do lado do servidor incluem a configuração de PKI para distribuir os certificados usados pelos usuários, o servidor VPN e o servidor NPS.  Você também pode configurar o RRAS para dar suporte a conexões IKEv2 e o servidor NPS para executar a autorização para as conexões VPN.

Para configurar a infraestrutura de servidor, você deve executar as seguintes tarefas:

- **Em um servidor configurado com o Active Directory Domain Services:** Habilitar o registro automático de certificados na diretiva de grupo para computadores e usuários, crie o grupo de usuários de VPN, o grupo de servidores VPN e o grupo de servidores NPS e adicionar membros a cada grupo.
- **Em um servidor de certificados do Active Directory da autoridade de certificação:** Crie os modelos de certificado de autenticação de usuário, autenticação do servidor VPN e autenticação do servidor NPS.
- **Em clientes do Windows 10 ingressados no domínio:** Registrar e validar certificados do usuário.

## <a name="step-3-configure-the-remote-access-server-for-always-on-vpnvpn-deploy-rasmd"></a>[Etapa 3. Configurar o servidor de acesso remoto da VPN Always On](vpn-deploy-ras.md)

Nesta etapa, você deve configurar VPN de acesso remoto para permitir conexões de VPN IKEv2, negar conexões de outros protocolos VPN e atribuir um pool de endereços IP estáticos para a emissão de endereços IP para se conectar a clientes autorizados de VPN.

Para configurar o acesso remoto, você deve executar as seguintes tarefas:

- Registrar e validar o certificado do servidor VPN
- Instalar e configurar o acesso remoto VPN

## <a name="step-4-install-and-configure-the-nps-servervpn-deploy-npsmd"></a>[Etapa 4. Instalar e configurar o servidor NPS](vpn-deploy-nps.md)

Nesta etapa, instale o servidor de diretivas de rede (NPS) usando o Windows PowerShell ou o Server Manager adicionar funções e Assistente de recursos. Você também pode configurar o NPS para lidar com todos os autenticação, autorização e obrigações de estatísticas de solicitação de conexão que ele recebe do servidor VPN.

Para configurar o NPS, você deve executar as seguintes tarefas:

- Registrar o servidor NPS no Active Directory
- Configurar RADIUS estatísticas para o servidor NPS
- Adicione o servidor VPN como um cliente RADIUS no NPS
- Configurar a política de rede no NPS
- Registrar automaticamente o certificado do servidor NPS

## <a name="step-5-configure-dns-and-firewall-settings-for-always-on-vpnvpn-deploy-dns-firewallmd"></a>[Etapa 5. Configurar o DNS e configurações de Firewall para sempre na VPN](vpn-deploy-dns-firewall.md)

Nesta etapa, você configura o DNS e o Firewall configurações. Quando se conectar a clientes remotos VPN, eles usam os mesmos servidores DNS que usam seus clientes internos, que lhes permite resolver os nomes da mesma maneira que o restante das suas estações de trabalho internos. 

## <a name="step-6-configure-windows-10-client-always-on-vpn-connectionsvpn-deploy-client-vpn-connectionsmd"></a>[Etapa 6. Configurar as conexões da VPN Always On do cliente Windows 10](vpn-deploy-client-vpn-connections.md)

Nesta etapa, você deve configurar os computadores de cliente do Windows 10 para se comunicar com que a infraestrutura com uma conexão VPN. Você pode usar várias tecnologias para configurar clientes de VPN do Windows 10, incluindo o Windows PowerShell, o System Center Configuration Manager e o Intune. Todos os três exigem um perfil VPN XML para definir as configurações de VPN apropriadas.

## <a name="step-7-optional-configure-conditional-access-for-vpn-connectivityad-ca-vpn-connectivity-windows10md"></a>[Etapa 7. (Opcional) Configurar o acesso condicional para conectividade VPN](../../ad-ca-vpn-connectivity-windows10.md)

Nesta etapa opcional, você pode ajustar como autorizado VPN aos usuários acessam seus recursos. Com acesso condicional do Azure AD para conectividade VPN, você pode ajudar a proteger as conexões VPN. Acesso condicional é um mecanismo de avaliação com base em política que permite que você crie regras de acesso para qualquer AD do Azure conectada aplicativo. Para obter mais informações, consulte [acesso condicional do Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).

## <a name="next-step"></a>Próximas etapas

[Etapa 1. Planejar a implantação de VPN Always On](always-on-vpn-deploy-planning.md): Antes de instalar a função de servidor de acesso remoto no computador que você estiver planejando usar como um servidor VPN. Após o planejamento adequado, você pode implantar a VPN Always On e, opcionalmente, configure o acesso condicional para conectividade VPN usando o Azure AD.  
