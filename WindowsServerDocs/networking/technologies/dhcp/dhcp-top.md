---
title: Protocolo DHCP (protocolo DHCP)
description: Este tópico fornece uma breve visão geral de DHCP Dynamic Host Configuration Protocol () no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: article
ms.assetid: 0ff29ef3-c458-4432-9065-e50a7de5b4b9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 770cfd78c7b9a0e122bd9f9936623a73b56af808
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="dynamic-host-configuration-protocol-dhcp"></a>Protocolo DHCP (protocolo DHCP)

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para obter uma visão geral do DHCP no Windows Server 2016.

>[!NOTE]
>Além de neste tópico, a seguinte documentação DHCP está disponível.
>
>- [O que há de novo no DHCP](What-s-New-in-DHCP.md)
>- [Implantar DHCP usando o Windows PowerShell](dhcp-deploy-wps.md)

Dynamic Host Configuration Protocol DHCP () é um protocolo de cliente/servidor que fornece automaticamente um host de protocolo de Internet (IP) com seu endereço IP e outras informações de configuração relacionados, como a máscara de sub-rede, os gateway padrão. As RFCs 2131 e 2132 definem DHCP como uma Internet Engineering Task Force (IETF) padrão com base em Bootstrap Protocol (BOOTP), um protocolo com o qual o DHCP compartilha muitos detalhes de implementação. DHCP permite que hosts obter informações de configuração necessárias TCP/IP de um servidor DHCP.

Windows Server 2016 inclui servidor DHCP, que é uma função de servidor de rede opcional que você pode implantar em sua rede para concedem endereços IP e outras informações para clientes DHCP. Todos os sistemas operacionais de cliente baseado no Windows incluem o cliente DHCP como parte de TCP/IP e cliente DHCP é habilitada por padrão.

## <a name="why-use-dhcp"></a>Por que usar DHCP?

Cada dispositivo em uma rede baseada em TCP/IP deve ter um endereço IP unicast exclusivo para acessar a rede e seus recursos. Sem o DHCP, endereços IP para computadores novos ou que são movidos de uma sub-rede para outro devem ser configurados manualmente; Endereços IP para computadores que são removidos da rede devem ser recuperados manualmente.

Com o DHCP, todo esse processo é automatizado e gerenciado centralmente. O servidor DHCP mantém um pool de endereços IP e concede um endereço para qualquer cliente DHCP habilitado ao ser iniciado na rede. Como os endereços IP são dinâmicos (concedidos) em vez de estático (permanentemente atribuído), endereços não mais em uso são automaticamente retornados ao pool para realocação.

O administrador da rede estabelece servidores DHCP que mantêm as informações de configuração de TCP/IP e fornecem configuração endereço aos clientes DHCP na forma de uma oferta de concessão. O servidor DHCP armazena as informações de configuração em um banco de dados que inclui:

- Parâmetros de configuração de TCP/IP válidos para todos os clientes na rede.

- Endereços IP válidos, mantidos em um pool para atribuição a clientes, bem como excluído endereços.

- Endereços IP reservados associados com clientes DHCP específicos. Isso permite que a atribuição consistente de um único endereço IP para um único cliente DHCP.

- A duração da concessão ou o período de tempo para o qual o endereço IP pode ser usado antes que uma renovação de concessão seja necessária.

Um cliente DHCP ativado após aceitar uma oferta de concessão, recebe:

- Um endereço IP válido para a sub-rede ao qual ele está se conectando.  
  
- Solicitado opções de DHCP, que são parâmetros adicionais que um servidor DHCP está configurado para atribuir a clientes. Alguns exemplos das opções de DHCP são roteador (gateway padrão), servidores DNS e nome de domínio DNS.

## <a name="benefits-of-dhcp"></a>Benefícios do DHCP

O DHCP fornece os seguintes benefícios.

- **Configuração de endereço IP confiável**. DHCP minimiza erros de configuração causados pela configuração manual de endereço IP, como erros de digitação ou para resolver conflitos causados pela atribuição de um endereço IP a ser mais de um computador ao mesmo tempo.

- **Redução da administração de rede**. DHCP inclui os seguintes recursos para reduzir a administração de rede:

    - Configuração centralizada e automatizada de TCP/IP.

    - A capacidade de definir configurações de TCP/IP de um local central.

    - A capacidade de atribuir uma ampla gama de valores de configuração de TCP/IP adicionais por meio de opções de DHCP.

    - A manipulação eficiente de endereço IP muda para clientes que devem ser atualizados com frequência, como aqueles para dispositivos portáteis que se movem para locais diferentes em uma rede sem fio.

    - O encaminhamento de mensagens DHCP iniciais usando um agente de retransmissão DHCP, que elimina a necessidade de um servidor DHCP em cada sub-rede.

