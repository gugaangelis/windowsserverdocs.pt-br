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
ms.openlocfilehash: d629f04abda0ce22deb75e487f5b485f50a60a53
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6066969"
---
# Etapa 1. Planejar a implantação de VPN Always On

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10


& #171;  [ **Anterior:** Saiba mais sobre o fluxo de trabalho para a implantação de VPN Always On](always-on-vpn-deploy-deployment.md)<br>
& #187;  [ **Próximo:** etapa 2. Configurar a infraestrutura de servidor](vpn-deploy-server-infrastructure.md)

Nesta etapa, você deve começar a planejar e preparar a implantação de VPN Always On. Antes de instalar a função de servidor de acesso remoto no computador que você estiver planejando usar como um servidor VPN, execute as seguintes tarefas. Após o planejamento adequado, você pode implantar VPN Always On e, opcionalmente, configurar o acesso condicional para conectividade VPN usando o Azure AD. 

[!INCLUDE [always-on-vpn-standard-config-considerations-include](../../../includes/always-on-vpn-standard-config-considerations-include.md)]


## Preparar o servidor de acesso remoto

Você deve fazer o seguinte no computador usado como um servidor VPN: 

- **Verifique se o software de servidor VPN e configuração de hardware está correta**. Instale o Windows Server 2016 no computador que você pretende usar como um servidor de acesso remoto VPN. Esse servidor deve ter dois adaptadores de rede física instalados, um para se conectar à rede de perímetro externo e um para se conectar à rede de perímetro interno.

- **Identifique qual adaptador de rede se conecta à Internet e qual adaptador de rede se conecta ao seu privada rede**. Configure o adaptador de rede conectados à Internet com um endereço IP público, enquanto o adaptador voltado para a Intranet pode usar um endereço IP da rede local.

    >[!TIP]
    >Se você preferir não usar um endereço IP público em sua rede de perímetro, você pode configurar o Firewall do Edge com um endereço IP público e, em seguida, configurar o firewall para encaminhar solicitações de conexão de VPN para o servidor VPN.

- **Conectar-se o servidor VPN à rede**. Instale o servidor VPN em uma rede de perímetro, entre o firewall edge e o firewall do perímetro.

## Planejar métodos de autenticação

IKEv2 é uma VPN descrito na [Internet Engineering Task Force solicitar Comments7296](https://datatracker.ietf.org/doc/rfc7296/)de protocolo de encapsulamento. A principal vantagem do IKEv2 é que ele tolera interrupções na conexão de rede subjacente. Por exemplo, se uma perda temporária na conexão ou se um usuário move um computador cliente da rede de um para outro, quando restabelecer a conexão de rede IKEv2 restaura a conexão VPN automaticamente — sem intervenção do usuário.

>[!TIP]
>Você pode configurar o servidor de acesso remoto VPN para dar suporte a conexões IKEv2 enquanto também desabilitando protocolos não utilizados, o que reduz o volume de segurança do servidor. 

## Planejar endereços IP para clientes remotos

Você pode configurar o servidor VPN para atribuir endereços para clientes VPN de um pool de endereço estático que você definir ou endereços IP de um servidor DHCP. 

## Preparar o ambiente

- **Certifique-se de que você tem permissões para configurar seu firewall externo e se você tem um endereço IP público válido**. Portas abertas no firewall para dar suporte a conexões VPN IKEv2. Você também precisa de um endereço IP público para aceitar conexões de clientes externos.

- **Escolha um intervalo de endereços IP estáticos para clientes VPN**. Determine o número máximo de clientes VPN simultâneos que você deseja dar suporte. Além disso, planeje um intervalo de endereços IP estáticos na rede de perímetro interno para atender a esse requisito, ou seja, o *pool de endereço estático*. Se você usar o DHCP para fornecer endereços IP no DMZ interno, você pode também precisa criar uma exclusão para esses endereços IP estáticos em DHCP.

- **Certifique-se de que você pode editar sua zona DNS pública**. Adicione registros DNS para seu domínio DNS público para dar suporte a infraestrutura VPN. 

- **Certifique-se de que todos os usuários VPN têm contas de usuário no Active Directory User \(AD DS\)**. Antes dos usuários podem se conectar à rede com conexões VPN, eles devem ter contas de usuário no ADDS.

## Preparar o roteamento e de Firewall 

Instale o servidor VPN dentro da rede de perímetro, que divide a rede de perímetro em redes de perímetro internos e externos. Dependendo do ambiente de rede, você precisa fazer várias modificações de roteamento.

- **Encaminhamento de porta de configurar \(Optional\).** Seu firewall edge deve abrir as portas e protocolos identificações associadas a uma VPN IKEv2 e encaminhá-los para o servidor VPN. Na maioria dos ambientes, fazendo assim exige que você configurar o encaminhamento de porta. Redirecione portas Universal (protocolo UDP) 500 e 4500 para o servidor VPN.

- **Configurar roteamento de modo que os servidores DNS e servidores VPN podem acessar a Internet**. Essa implantação usa IKEv2 e conversão de endereços de rede \(NAT\). Certifique-se de que o servidor VPN pode acessar todas as redes internas necessárias e recursos de rede. Qualquer rede ou recurso inacessível do servidor VPN também está inacessível em conexões VPN de locais remotos.

Na maioria dos ambientes, para alcançar a nova rede de perímetro interno, ajuste rotas estáticas no firewall do edge e o servidor VPN. Em ambientes mais complexos, no entanto, talvez seja necessário adicionar rotas estáticas à roteadores internos ou ajustar as regras de firewall interno para o servidor VPN e o bloco de endereços IP associados aos clientes VPN.

## Próximas etapas
[Etapa 2. Configurar a infraestrutura de servidor](vpn-deploy-server-infrastructure.md): nesta etapa, você pode instalar e configurar os componentes de servidor necessários para dar suporte a VPN. Os componentes de servidor incluem a configuração de PKI para distribuir os certificados usados pelos usuários, o servidor VPN e o servidor NPS. 

---