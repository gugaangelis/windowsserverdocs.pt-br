---
title: RDS – Executar e ajustar o ambiente de Serviços de Área de Trabalho Remota
description: Fornece informações de gerenciamento de Serviços de Área de Trabalho Remota.
ms.author: spatnaik
ms.date: 02/08/2017
ms.topic: article
ms.assetid: 79909767-a4c3-4ecf-8d3f-77d37a663153
author: spatnaik
manager: scottman
ms.openlocfilehash: 2b7443255f5490a0b67633abbc9ea5bbb6a5bb82
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954883"
---
# <a name="run-and-tune-your-remote-desktop-services-environment"></a>Executar e ajustar seu ambiente de Serviços de Área de Trabalho Remota

O ajuste da implantação leva tempo e exige instrumentação e monitoramento. Use os processos a seguir para refinar sua implantação de Área de Trabalho Remota, mantê-la em execução e habilitar o dimensionamento horizontal (e vertical), conforme necessário.

É uma boa prática avaliar continuamente as métricas e o saldo em relação aos custos de execução.

## <a name="management-and-monitoring"></a>Gerenciamento e monitoramento

Confira [Gerenciar usuários na coleção de RDS](rds-user-management.md) para obter informações sobre como gerenciar o acesso a áreas de trabalho e recursos remotos.

Use o **OMS (Microsoft Operations Management Suite)** para monitorar implantações de Área de Trabalho Remota quanto a possíveis gargalos e gerenciá-los de uma das seguintes maneiras:

- **Gerenciador do Servidor**: Use a ferramenta de gerenciamento de Área de Trabalho Remota interna ao Windows Server para gerenciar implantações com até 500 usuários finais remotos simultâneos.
- **PowerShell**: Use o módulo do PowerShell de Área de Trabalho Remota, também interno ao Windows Server, para gerenciar implantações com até 5.000 usuários finais remotos simultâneos.

## <a name="scale-bigger-better-faster"></a>Escala: Quanto maior, melhor e mais rápido

Com visibilidade sobre a implantação, você pode controlar a escala com mais precisão. Adicione ou remova com facilmente servidores de host de Área de Trabalho Remota com base nas necessidades de escala.

As implantações de Área de Trabalho Remota criadas no Azure podem fazer uso de serviços do Azure, como SQL Azure, para dimensionar automaticamente sob demanda.

## <a name="automation-script-for-success"></a>Automação: Script para o sucesso

Manter um aplicativo em execução e altamente dimensionado envolve a repetição de operações regularmente. Use cmdlets do PowerShell dos Serviços de Área de Trabalho Remota e provedores WMI para desenvolver scripts que podem ser executados em várias implantações quando necessário. Execute as regras do BPA (Analisador de Práticas Recomendadas) para os Serviços de Área de Trabalho Remota em suas implantações para ajustá-las.

## <a name="load-testing-avoid-surprises"></a>Teste de carga: Evitar surpresas

Faça um teste de carga da implantação com testes de estresse e simulação do uso na vida real. Varie o tamanho da carga para evitar surpresas. Verifique se a capacidade de resposta atende aos requisitos do usuário e se todo o sistema é resiliente. Crie testes de carga com ferramentas de simulação, como LoginVSI, que verifica a capacidade da implantação para atender às necessidades dos usuários.