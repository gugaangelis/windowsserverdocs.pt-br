---
title: Planejar a implantação de VPN Always On
description: Este tópico fornece instruções de planejamento para a implantação de Always On VPN no Windows Server 2016.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 3c9de3ec-4bbd-4db0-b47a-03507a315383
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 11/05/2018
ms.openlocfilehash: f92cfdbe13633dd4c59012f566c6888fdc7fc7a1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388163"
---
# <a name="step-1-plan-the-always-on-vpn-deployment"></a>Etapa 1. Planejar a implantação de VPN Always On

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior** Saiba mais sobre o fluxo de trabalho para implantar Always On VPN](always-on-vpn-deploy-deployment.md)
- [**Última** Etapa 2. Configurar a infraestrutura do servidor](vpn-deploy-server-infrastructure.md)

Nesta etapa, você começa a planejar e preparar sua implantação de VPN Always On. Antes de instalar a função de servidor de acesso remoto no computador que você está planejando usar como um servidor VPN, execute as seguintes tarefas. Após o planejamento adequado, você pode implantar Always On VPN e, opcionalmente, configurar o acesso condicional para conectividade VPN usando o Azure AD.

[!INCLUDE [always-on-vpn-standard-config-considerations-include](../../../includes/always-on-vpn-standard-config-considerations-include.md)]

## <a name="prepare-the-remote-access-server"></a>Preparar o servidor de acesso remoto

Você deve fazer o seguinte no computador usado como um servidor VPN:

- **Verifique se a configuração de software e hardware do servidor VPN está correta**. Instale o Windows Server 2016 no computador que você planeja usar como um servidor VPN de acesso remoto. Esse servidor deve ter dois adaptadores de rede física instalados, um para se conectar à rede de perímetro externa e outro para se conectar à rede de perímetro interna.

- **Identifique qual adaptador de rede se conecta à Internet e qual adaptador de rede se conecta à sua rede privada**. Configure o adaptador de rede voltado para a Internet com um endereço IP público, enquanto o adaptador voltado para a intranet pode usar um endereço IP da rede local.

    >[!TIP]
    >Se preferir não usar um endereço IP público em sua rede de perímetro, você poderá configurar o Firewall do Edge com um endereço IP público e, em seguida, configurar o firewall para encaminhar solicitações de conexão VPN para o servidor VPN.

- **Conecte o servidor VPN à rede**. Instale o servidor VPN em uma rede de perímetro, entre o Firewall do Edge e o Firewall do perímetro.

## <a name="plan-authentication-methods"></a>Planejar métodos de autenticação

IKEv2 é um protocolo de encapsulamento de VPN descrito em [solicitação de força de tarefa de engenharia da Internet para comentários 7296](https://datatracker.ietf.org/doc/rfc7296/). A principal vantagem do IKEv2 é que ele tolera interrupções na conexão de rede subjacente. Por exemplo, se uma perda temporária na conexão ou se um usuário mover um computador cliente de uma rede para outra, ao restabelecer a conexão de rede, o IKEv2 restaura a conexão VPN automaticamente, sem a intervenção do usuário.

>[!TIP]
>Você pode configurar o servidor VPN de acesso remoto para dar suporte a conexões IKEv2 e também desabilitar protocolos não utilizados, o que reduz a superfície de segurança do servidor. 

## <a name="plan-ip-addresses-for-remote-clients"></a>Planejar endereços IP para clientes remotos

Você pode configurar o servidor VPN para atribuir endereços a clientes VPN de um pool de endereços estáticos que você configura ou endereços IP de um servidor DHCP. 

## <a name="prepare-the-environment"></a>Preparar o ambiente

- **Verifique se você tem permissões para configurar o firewall externo e se tem um endereço IP público válido**. Abra portas no firewall para dar suporte a conexões VPN IKEv2. Você também precisa de um endereço IP público para aceitar conexões de clientes externos.

- **Escolha um intervalo de endereços IP estáticos para clientes VPN**. Determine o número máximo de clientes VPN simultâneos aos quais você deseja dar suporte. Além disso, planeje um intervalo de endereços IP estáticos na rede de perímetro interna para atender a esse requisito, ou seja, o *pool de endereços estáticos*. Se você usar o DHCP para fornecer endereços IP na DMZ interna, também poderá ser necessário criar uma exclusão para esses endereços IP estáticos no DHCP.

- **Verifique se você pode editar sua zona DNS pública**. Adicione registros DNS ao seu domínio DNS público para dar suporte à infraestrutura de VPN. 

- **Certifique-se de que todos os usuários VPN tenham contas de usuário no Active Directory usuário (AD DS)** . Antes que os usuários possam se conectar à rede com conexões VPN, eles devem ter contas de usuário no AD DS.

## <a name="prepare-routing-and-firewall"></a>Preparar o roteamento e o firewall 

Instale o servidor VPN dentro da rede de perímetro, que particiona a rede de perímetro em redes de perímetro internas e externas. Dependendo do seu ambiente de rede, talvez seja necessário fazer várias modificações de roteamento.

- **Adicional Configure o encaminhamento de porta.** O Firewall do Edge deve abrir as portas e as IDs de protocolo associadas a uma VPN IKEv2 e encaminhá-las ao servidor VPN. Na maioria dos ambientes, fazer isso exige que você configure o encaminhamento de porta. Redirecione as portas 500 e 4500 do protocolo UDP (Universal Datagram Protocol) para o servidor VPN.

- **Configure o roteamento para que os servidores DNS e os servidores VPN possam acessar a Internet**. Essa implantação usa IKEv2 e NAT (conversão de endereços de rede). Verifique se o servidor VPN pode alcançar todas as redes internas e os recursos de rede necessários. Qualquer rede ou recurso inacessível do servidor VPN também está inacessível em conexões VPN de locais remotos.

Na maioria dos ambientes, para alcançar a nova rede de perímetro interna, ajuste as rotas estáticas no firewall de borda e no servidor VPN. Em ambientes mais complexos, no entanto, talvez seja necessário adicionar rotas estáticas a roteadores internos ou ajustar as regras internas de firewall para o servidor VPN e o bloco de endereços IP associados a clientes VPN.

## <a name="next-steps"></a>Próximas etapas

[Etapa 2. Configurar a infraestrutura](vpn-deploy-server-infrastructure.md)do servidor: Nesta etapa, você instala e configura os componentes do lado do servidor necessários para dar suporte à VPN. Os componentes do lado do servidor incluem a configuração de PKI para distribuir os certificados usados pelos usuários, o servidor VPN e o servidor NPS.