---
title: Protocolo DHCP
description: Este tópico fornece uma breve visão geral do protocolo DHCP no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dhcp
ms.topic: article
ms.assetid: 0ff29ef3-c458-4432-9065-e50a7de5b4b9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c71517bc742cf9eda62cc7d83128f1ab9bd04547
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355401"
---
# <a name="dynamic-host-configuration-protocol-dhcp"></a>Protocolo DHCP

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para obter uma breve visão geral do DHCP no Windows Server 2016.

> [!NOTE]
> Além deste tópico, a seguinte documentação do DHCP está disponível.
>
> - [O que há de novo no DHCP](What-s-New-in-DHCP.md)
> - [Implantar o DHCP usando o Windows PowerShell](dhcp-deploy-wps.md)

O protocolo DHCP é um protocolo de cliente/servidor que fornece automaticamente um host de IP (Internet Protocol) com seu endereço IP e outras informações de configuração relacionadas, como a máscara de sub-rede e o gateway padrão. As RFCs 2131 e 2132 definem o DHCP como um padrão IETF (Internet Engineering Task Force) baseado no protocolo BOOTP, um protocolo com o qual o DHCP compartilha muitos detalhes de implementação. O DHCP permite que os hosts obtenham as informações de configuração de TCP/IP necessárias de um servidor DHCP.

O Windows Server 2016 inclui o servidor DHCP, que é uma função de servidor de rede opcional que você pode implantar em sua rede para conceder endereços IP e outras informações para clientes DHCP. Todos os sistemas operacionais cliente baseados no Windows incluem o cliente DHCP como parte do TCP/IP, e o cliente DHCP é habilitado por padrão.

## <a name="why-use-dhcp"></a>Por que usar o DHCP?

Cada dispositivo em uma rede baseada em TCP/IP deve ter um endereço IP unicast exclusivo para acessar a rede e seus recursos. Sem DHCP, os endereços IP para novos computadores ou computadores movidos de uma sub-rede para outra devem ser configurados manualmente; Os endereços IP para computadores que são removidos da rede devem ser recuperados manualmente.

Com o DHCP, todo esse processo é automatizado e gerenciado centralmente. O servidor DHCP mantém um pool de endereços IP e concede um endereço a qualquer cliente habilitado para DHCP quando ele é iniciado na rede. Como os endereços IP são dinâmicos (concedidos) em vez de estáticos (permanentemente atribuídos), os endereços que não estão mais em uso são retornados automaticamente para o pool para realocação.

O administrador de rede estabelece servidores DHCP que mantêm informações de configuração de TCP/IP e fornecem a configuração de endereço para clientes habilitados para DHCP na forma de uma oferta de concessão. O servidor DHCP armazena as informações de configuração em um banco de dados que inclui:

- Parâmetros de configuração de TCP/IP válidos para todos os clientes na rede.

- Endereços IP válidos, mantidos em um pool para atribuição a clientes, bem como endereços excluídos.

- IP Reservado endereços associados a clientes DHCP específicos. Isso permite uma atribuição consistente de um único endereço IP a um único cliente DHCP.

- A duração da concessão ou o período de tempo pelo qual o endereço IP pode ser usado antes que uma renovação de concessão seja necessária.

Um cliente habilitado para DHCP, após aceitar uma oferta de concessão, recebe:

- Um endereço IP válido para a sub-rede à qual ele está se conectando.  
  
- Opções de DHCP solicitadas, que são parâmetros adicionais que um servidor DHCP está configurado para atribuir a clientes. Alguns exemplos de opções de DHCP são roteador (gateway padrão), servidores DNS e nome de domínio DNS.

## <a name="benefits-of-dhcp"></a>Benefícios do DHCP

O DHCP fornece os seguintes benefícios.

- **Configuração de endereço IP confiável**. O DHCP minimiza os erros de configuração causados pela configuração manual do endereço IP, como erros tipográficos ou conflitos de endereços causados pela atribuição de um endereço IP a mais de um computador ao mesmo tempo.

- **Administração de rede reduzida**. O DHCP inclui os seguintes recursos para reduzir a administração da rede:

    - Configuração de TCP/IP centralizada e automatizada.

    - A capacidade de definir configurações de TCP/IP a partir de um local central.

    - A capacidade de atribuir uma gama completa de valores de configuração TCP/IP adicionais por meio de opções de DHCP.

    - O tratamento eficiente de alterações de endereço IP para clientes que devem ser atualizados com frequência, como aqueles para dispositivos portáteis que se movem para locais diferentes em uma rede sem fio.

    - O encaminhamento de mensagens do DHCP inicial usando um agente de retransmissão DHCP, que elimina a necessidade de um servidor DHCP em cada sub-rede.

