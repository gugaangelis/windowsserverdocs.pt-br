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
ms.openlocfilehash: 15c60f2986d3837c5c6e03f9e0a25c7e0a4e0cc0
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6066971"
---
# Implantar VPN Always On

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #0171; [ **Anterior:** Saiba mais sobre a VPN Always On recursos avançados](always-on-vpn-adv-options.md)<br>
& #0187; [ **Próximo:** etapa 1. Começar a planejar a implantação de VPN Always On](always-on-vpn-deploy-planning.md)

Nesta seção, você saberá mais sobre o fluxo de trabalho para implantar conexões VPN Always On para computadores cliente de Windows 10 de domínio remotos. Se você deseja **Configurar o acesso condicional** ajustar como os usuários da VPN acessam seus recursos, consulte [o acesso condicional para conectividade VPN usando o Azure AD](../../ad-ca-vpn-connectivity-windows10.md). Para saber mais sobre acesso condicional para conectividade VPN usando o Azure AD, consulte [o acesso condicional no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). 


O diagrama a seguir ilustra o processo de fluxo de trabalho para os diferentes cenários durante a implantação de VPN Always On: 

_Clique na imagem para ampliar_.

<a href="../../../../media/Always-On-Vpn/always-on-vpn-deployment-workflow.png" alt="Full-sized view of the Always On VPN deployment workflow" target="_blank">![miniatura](../../../../media/Always-On-Vpn/always-on-vpn-deployment-workflow-sm.png)
</a> 

>[!IMPORTANT]
>Para essa implantação, não é um requisito de que os servidores de infraestrutura, como computadores que executam o Active Directory Domain Services, serviços de certificados do Active Directory e servidor de políticas de rede, estão executando o Windows Server 2016. Você pode usar as versões anteriores do Windows Server, como Windows Server 2012 R2, para os servidores de infraestrutura e para o servidor que está executando o acesso remoto.

## [Etapa 1. Planejar a implantação de VPN Always On](always-on-vpn-deploy-planning.md)

Nesta etapa, você deve começar a planejar e preparar a implantação de VPN Always On. Antes de instalar a função de servidor de acesso remoto no computador que você estiver planejando usar como um servidor VPN. Após o planejamento adequado, você pode implantar VPN Always On e, opcionalmente, configurar o acesso condicional para conectividade VPN usando o Azure AD.

## [Etapa 2. Configurar a infraestrutura de servidor VPN Always On](vpn-deploy-server-infrastructure.md)

Nesta etapa, você instala e configurar os componentes de servidor necessários para dar suporte a VPN. Os componentes de servidor incluem a configuração de PKI para distribuir os certificados usados pelos usuários, o servidor VPN e o servidor NPS.  Você também pode configurar RRAS para dar suporte a conexões IKEv2 e o servidor NPS para executar a autorização para as conexões VPN.

Para configurar a infraestrutura de servidor, você deve executar as seguintes tarefas:
- **Em um servidor configurado com os serviços de domínio do Active Directory:** Habilitar o registro automático de certificado na política de grupo para usuários e computadores, criar o grupo de usuários de VPN, o grupo de servidores VPN e o grupo de servidores NPS e adicionar membros para cada grupo.
- **Em um servidor de certificados do Active Directory CA:** Crie os modelos de certificado de autenticação do usuário, autenticação de servidor VPN e autenticação do servidor NPS.
- **Em clientes do Windows 10 ingressado no domínio:** Registrar e validar certificados de usuário.

## [Etapa 3. Configurar o servidor de acesso remoto para VPN Always On](vpn-deploy-ras.md)

Nesta etapa, você deve configurar acesso remoto VPN para permitir conexões VPN IKEv2, negar conexões de outros protocolos VPN e atribuir um pool de endereços IP estáticos para a emissão de endereços IP para clientes VPN autorizados conectados.

Para configurar o RAS, você deve executar as seguintes tarefas:
- Inscrever-se e validar o certificado do servidor VPN
- Instalar e configurar o acesso remoto VPN

## [Etapa 4. Instalar e configurar o servidor NPS](vpn-deploy-nps.md)

Nesta etapa, você instalar o servidor de política de rede (NPS) usando o Windows PowerShell ou o Server Manager adicionar funções e recursos do assistente. Você também pode configurar o NPS para manipular todas as obrigações de estatísticas de solicitação de conexão que ele recebe do servidor VPN, autorização e autenticação.

Para configurar o NPS, você deve executar as seguintes tarefas:
- Registrar o servidor NPS no Active Directory
- Configurar RADIUS estatísticas do seu servidor NPS
- Adicione o servidor VPN como um cliente RADIUS no NPS
- Configurar política de rede no NPS
- Registrar automaticamente o certificado do servidor NPS

## [Etapa 5. Configurar o DNS e configurações de Firewall para sempre em VPN](vpn-deploy-dns-firewall.md)

Nesta etapa, você configura DNS e o Firewall configurações. Quando os clientes remotos VPN se conecta, eles usam os mesmos servidores DNS que usam a seus clientes internos, que permite que eles resolver nomes da mesma maneira como o restante do seus estações de trabalho internas. 

## [Etapa 6. Configurar conexões de VPN Always On em cliente do Windows 10](vpn-deploy-client-vpn-connections.md)

Nesta etapa, você deve configurar os computadores de cliente do Windows 10 para se comunicar com essa infraestrutura com uma conexão VPN. Você pode usar várias tecnologias para configurar clientes VPN do Windows 10, incluindo o Windows PowerShell, System Center Configuration Manager e Intune. Todos os três exigem um perfil de VPN XML para definir as configurações de VPN apropriadas. 

## [Etapa 7. (Opcional) Configurar o acesso condicional para conectividade VPN](../../ad-ca-vpn-connectivity-windows10.md) 
Nesta etapa opcional, você pode ajustar VPN autorizado como os usuários acessam os recursos. Com acesso condicional do Azure AD para conectividade VPN, você pode ajudar a proteger as conexões VPN. Acesso condicional é um mecanismo de avaliação com base em política que permite que você crie regras de acesso para qualquer Azure AD conectado ao aplicativo. Para obter mais informações, consulte o [acesso condicional do Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).


## Próximas etapas
[Etapa 1. Planejar a implantação de VPN Always On](always-on-vpn-deploy-planning.md): antes de instalar a função de servidor de acesso remoto no computador que você estiver planejando usar como um servidor VPN. Após o planejamento adequado, você pode implantar VPN Always On e, opcionalmente, configurar o acesso condicional para conectividade VPN usando o Azure AD.  



---