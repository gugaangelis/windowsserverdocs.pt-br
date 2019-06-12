---
title: Protocolo DHCP
description: Este tópico fornece uma visão geral da configuração de protocolo DHCP (Dynamic Host) no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: article
ms.assetid: 0ff29ef3-c458-4432-9065-e50a7de5b4b9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7828d75d58ff328e826cb685899a76347ce56953
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812210"
---
# <a name="dynamic-host-configuration-protocol-dhcp"></a>Protocolo DHCP

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para obter uma visão geral do DHCP no Windows Server 2016.

> [!NOTE]
> Além deste tópico, a seguinte documentação de DHCP está disponível.
>
> - [O que há de novo no DHCP](What-s-New-in-DHCP.md)
> - [Implantar o DHCP usando o Windows PowerShell](dhcp-deploy-wps.md)

Protocolo de configuração de Host dinâmico (DHCP) é um protocolo de cliente/servidor que fornece automaticamente um host de IP (Internet Protocol) com seu endereço IP e outras informações de configuração relacionados como o gateway padrão e máscara de sub-rede. As RFCs 2131 e 2132 definem DHCP como uma Engineering Task Force IETF (Internet) padrão com base em Bootstrap Protocol (BOOTP), um protocolo com o qual o DHCP compartilha muitos detalhes de implementação. DHCP permite que os hosts obter informações de configuração necessárias do TCP/IP de um servidor DHCP.

Windows Server 2016 inclui um servidor DHCP, que é uma função de servidor de rede opcional que você pode implantar em sua rede para os endereços IP de concessão e outras informações para clientes DHCP. Todos os sistemas operacionais de cliente baseado em Windows incluem o cliente DHCP como parte do TCP/IP e cliente DHCP está habilitado por padrão.

## <a name="why-use-dhcp"></a>Por que usar o DHCP?

Todos os dispositivos em uma rede baseada em TCP/IP devem ter um endereço IP unicast exclusiva para acessar a rede e seus recursos. Sem o DHCP, os endereços IP para novos computadores ou computadores que são movidos de uma sub-rede para outra devem ser configurados manualmente; Endereços IP para computadores que são removidos da rede devem ser recuperados manualmente.

Com o DHCP, todo esse processo é automatizado e gerenciado centralmente. O servidor DHCP mantém um pool de endereços IP e concede um endereço para qualquer cliente habilitado para DHCP quando ele é iniciado na rede. Como os endereços IP são dinâmicos (concedidos) em vez de static (permanentemente atribuído), endereços não mais em uso são retornados automaticamente para o pool para a realocação.

O administrador da rede estabelece os servidores DHCP que mantém informações de configuração de TCP/IP e fornecem a configuração de endereço para clientes DHCP na forma de uma oferta de concessão. O servidor DHCP armazena as informações de configuração em um banco de dados que inclui:

- Parâmetros de configuração de TCP/IP válidos para todos os clientes na rede.

- Endereços IP válidos, mantidos em um pool para atribuição a clientes, bem como excluído de endereços.

- Endereços IP reservados associados com clientes do DHCP determinados. Isso permite que a atribuição consistente de um único endereço IP para um único cliente DHCP.

- A duração da concessão, ou o período de tempo que o endereço IP pode ser usado antes de uma renovação da concessão é necessária.

Um cliente DHCP habilitado, após a aceitar uma oferta de concessão, recebe:

- Um endereço IP válido para a sub-rede à qual ele está se conectando.  
  
- Solicitou opções de DHCP, que são parâmetros adicionais que um servidor DHCP está configurado para atribuir a clientes. Alguns exemplos de opções de DHCP são roteador (gateway padrão), servidores DNS e nome de domínio DNS.

## <a name="benefits-of-dhcp"></a>Benefícios do DHCP

O DHCP fornece os seguintes benefícios.

- **Configuração de endereço IP confiável**. DHCP minimiza erros de configuração causados pela configuração manual de endereço IP, como erros de digitação ou resolver conflitos causados pela atribuição de um endereço IP a mais de um computador ao mesmo tempo.

- **Redução da administração de rede**. DHCP inclui os seguintes recursos para reduzir a administração da rede:

    - Configuração automatizada e centralizada de TCP/IP.

    - A capacidade de definir as configurações de TCP/IP de um local central.

    - A capacidade de atribuir uma gama completa de valores de configuração de TCP/IP adicionais por meio de opções de DHCP.

    - A manipulação eficiente de alterações de endereço IP para clientes que devem ser atualizados com frequência, como aquelas para dispositivos portáteis que mover para locais diferentes em uma rede sem fio.

    - O encaminhamento de mensagens DHCP inicias por meio de um agente de retransmissão DHCP, que elimina a necessidade de um servidor DHCP em cada sub-rede.

