---
title: RDS - executar e ajustar
description: Fornece informações de gerenciamento de serviços de área de trabalho remota.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: spatnaik
ms.date: 02/08/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 79909767-a4c3-4ecf-8d3f-77d37a663153
author: spatnaik
manager: scottman
ms.openlocfilehash: 40f8dbd560da359e8764ed715e7776cc2d230a7f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862177"
---
# <a name="run-and-tune-your-remote-desktop-services-environment"></a>Executar e ajustar seu ambiente de serviços de área de trabalho remota

Ajuste a implantação leva tempo e exige a instrumentação e monitoramento. Use os processos a seguir para refinar sua implantação de área de trabalho remota, mantê-lo em execução e habilitar o dimensionamento (e), conforme necessário. 

É uma boa prática avaliar continuamente as métricas e saldo contra os custos de execução.

## <a name="management-and-monitoring"></a>Gerenciamento e monitoramento

Fazer cHeck-out [gerenciar usuários em sua coleção de RDS](rds-user-management.md) para obter informações sobre como gerenciar o acesso a suas áreas de trabalho e recursos remotos.

Use **Microsoft Operations Management Suite (OMS)** para monitorar implantações de área de trabalho remota para possíveis gargalos e gerenciá-los usando uma das seguintes maneiras: 

- **Gerenciador do servidor**: Use a ferramenta de gerenciamento de área de trabalho remota é interna ao Windows Server para gerenciar implantações com até 500 simultâneas remota aos usuários finais. 
- **PowerShell**: Use o módulo do PowerShell de área de trabalho remota, também é integrado ao Windows Server, para gerenciar implantações com até 5.000 simultâneas remota aos usuários finais.

## <a name="scale-bigger-better-faster"></a>Escala: Quanto maior, melhor e mais rápido

Visibilidade sobre a implantação, você pode controlar a escala com mais precisão. Facilmente adicionar ou remover servidores de host de área de trabalho remota com base nas necessidades de escala. 

Implantações de área de trabalho remota que são criadas no Azure podem fazer uso de serviços do Azure, como SQL Azure, para dimensionar automaticamente sob demanda.

## <a name="automation-script-for-success"></a>Automação: Script para o sucesso

Manter um aplicativo em execução, altamente dimensionado envolve a repetição de operações em uma base regular. Use cmdlets do PowerShell remoto de serviços de área de trabalho e provedores WMI para desenvolver scripts que podem ser executados em várias implantações quando necessário. Execute as regras do analisador de práticas recomendadas (BPA) para os serviços de área de trabalho remota em suas implantações para ajustar suas implantações.

## <a name="load-testing-avoid-surprises"></a>Teste de carga: Evitar surpresas

A implantação com testes de estresse e simulação do uso da vida real do teste de carga. Varie o tamanho de carga para evitar surpresas! Certifique-se de que a capacidade de resposta atende aos requisitos de usuário e que todo o sistema seja resiliente. Crie testes de carga com ferramentas de simulação, como LoginVSI, que verificar a capacidade da implantação para atender às necessidades dos usuários. 