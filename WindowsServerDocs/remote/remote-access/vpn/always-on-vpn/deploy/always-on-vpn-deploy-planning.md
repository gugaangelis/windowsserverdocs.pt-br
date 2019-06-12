---
title: Planejar a implantação de VPN Always On
description: Este tópico fornece instruções de planejamento para a implantação de VPN Always On no Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 3c9de3ec-4bbd-4db0-b47a-03507a315383
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 11/05/2018
ms.openlocfilehash: db309f451eb9187463f71dfd85a82d214c464e2b
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749454"
---
# <a name="step-1-plan-the-always-on-vpn-deployment"></a>Etapa 1. Planejar a implantação de VPN Always On

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Saiba mais sobre o fluxo de trabalho para a implantação de VPN Always On](always-on-vpn-deploy-deployment.md)
- [**Avançar:** Etapa 2. Configurar a infraestrutura de servidor](vpn-deploy-server-infrastructure.md)

Nesta etapa, você começar a planejar e preparar a implantação de VPN Always On. Antes de instalar a função de servidor de acesso remoto no computador que você estiver planejando usar como um servidor VPN, execute as seguintes tarefas. Após o planejamento adequado, você pode implantar a VPN Always On e, opcionalmente, configure o acesso condicional para conectividade VPN usando o Azure AD.

[!INCLUDE [always-on-vpn-standard-config-considerations-include](../../../includes/always-on-vpn-standard-config-considerations-include.md)]

## <a name="prepare-the-remote-access-server"></a>Preparar o servidor de acesso remoto

Você deve fazer o seguinte no computador usado como um servidor VPN:

- **Verifique se a configuração de hardware e software de servidor VPN está correta**. Instale o Windows Server 2016 no computador que você planeja usar como um servidor de acesso remoto VPN. Esse servidor deve ter dois adaptadores de rede físicos instalados, um para se conectar à rede de perímetro externo e um para se conectar à rede de perímetro interna.

- **Identificar qual adaptador de rede se conecta à Internet e qual adaptador de rede se conecta à sua rede privada**. Configure o adaptador de rede voltado para a Internet com um endereço IP público, enquanto o adaptador voltado para a Intranet pode usar um endereço IP da rede local.

    >[!TIP]
    >Se você preferir não usar um endereço IP público em sua rede de perímetro, você pode configurar o Firewall de borda com um endereço IP público e, em seguida, configure o firewall para encaminhar solicitações de conexão de VPN para o servidor VPN.

- **Conectar o servidor VPN à rede**. Instale o servidor VPN em uma rede de perímetro entre o firewall de borda e o firewall do perímetro.

## <a name="plan-authentication-methods"></a>Métodos de autenticação de plano

IKEv2 é uma VPN descrito de protocolo de encapsulamento [Internet Engineering Task Force solicitar comentários 7296](https://datatracker.ietf.org/doc/rfc7296/). A principal vantagem do IKEv2 é que ele tolera interrupções na conexão de rede subjacente. Por exemplo, se uma perda temporária na conexão ou se um usuário move um computador cliente de uma rede para outra, quando restabelecer a conexão de rede IKEv2 restaura a conexão VPN automaticamente — sem a intervenção do usuário.

>[!TIP]
>Você pode configurar o servidor de acesso remoto VPN para dar suporte a conexões IKEv2 enquanto também desabilitando protocolos não utilizados, o que reduz o volume de segurança do servidor. 

## <a name="plan-ip-addresses-for-remote-clients"></a>Planejar os endereços IP para clientes remotos

Você pode configurar o servidor VPN para atribuir endereços a clientes VPN de um pool de endereços estáticos que você configura ou endereços IP de um servidor DHCP. 

## <a name="prepare-the-environment"></a>Preparar o ambiente

- **Certifique-se de que você tenha permissões para configurar seu firewall externo e se você tem um endereço IP público válido**. Abrir portas no firewall para dar suporte a conexões VPN IKEv2. Você também precisa de um endereço IP público para aceitar conexões de clientes externos.

- **Escolha um intervalo de endereços IP estáticos para clientes VPN**. Determine o número máximo de clientes simultâneos de VPN que você deseja dar suporte. Além disso, planeje um intervalo de endereços IP estáticos na rede de perímetro interna para atender a esse requisito, ou seja, o *pool de endereços estáticos*. Se você usar o DHCP para fornecer endereços IP na rede de Perímetro interna, também convém criar uma exclusão para esses endereços IP estáticos no DHCP.

- **Certifique-se de que você pode editar sua zona DNS pública**. Adicione registros DNS para seu domínio DNS público para dar suporte a infraestrutura VPN. 

- **Verifique se todos os usuários VPN tem contas de usuário em um usuário do Active Directory (AD DS)** . Antes de se conectar à rede com conexões VPN, eles devem ter contas de usuário no AD DS.

## <a name="prepare-routing-and-firewall"></a>Preparar o roteamento e de Firewall 

Instale o servidor VPN dentro da rede de perímetro, que particiona a rede de perímetro em redes de perímetro interna e externa. Dependendo do seu ambiente de rede, você talvez precise fazer várias modificações de roteamentos.

- **(Opcional) Configure o encaminhamento de porta.** Seu firewall de borda deve abrir as portas e protocolos IDs associadas a uma VPN IKEv2 e encaminhá-los ao servidor VPN. Na maioria dos ambientes, fazendo assim exige que você configurar o encaminhamento de porta. Redirecione as portas 500 e 4500 do protocolo de datagrama Universal (UDP) para o servidor VPN.

- **Configurar o roteamento para que os servidores DNS e VPN pode acessar a Internet**. Essa implantação usa IKEv2 e conversão de endereço de rede (NAT). Certifique-se de que o servidor VPN pode acessar todas as redes internas necessárias e os recursos de rede. Qualquer recurso inacessível a partir do servidor VPN ou rede também está inacessível em conexões VPN de locais remotos.

Na maioria dos ambientes, para acessar a nova rede de perímetro interna, ajuste as rotas estáticas no firewall de borda e o servidor VPN. Em ambientes mais complexos, no entanto, você talvez precise adicionar rotas estáticas para roteadores internos ou ajustar as regras de firewall interno para o servidor VPN e o bloco de endereços IP associados a clientes VPN.

## <a name="next-steps"></a>Próximas etapas

[Etapa 2. Configurar a infraestrutura de servidor](vpn-deploy-server-infrastructure.md): Nesta etapa, você pode instala e configurar os componentes do lado do servidor necessários para oferecer suporte a VPN. Os componentes do lado do servidor incluem a configuração de PKI para distribuir os certificados usados pelos usuários, o servidor VPN e o servidor NPS.