---
title: Estudo de caso SDK do Windows Admin Center - Fujitsu
description: Estudo de caso SDK do Windows Admin Center - Fujitsu
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 05/23/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 6d916920b187dd3c637644a0f40ae9f9cca72b66
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814987"
---
# <a name="fujitsu-serverview-health-and-raid-extensions"></a>Extensões Fujitsu ServerView da integridade e RAID

## <a name="bringing-end-to-end-visibility-from-operating-system-to-hardware-into-windows-admin-center"></a>Levando a visibilidade de ponta a ponta, do sistema operacional para hardware, no Windows Admin Center

A Fujitsu é uma empresa de tecnologia informações e comunicações de japonês à esquerda e um fabricante de [PRIMERGY](http://www.fujitsu.com/fts/products/computing/servers/primergy/) e [PRIMEQUEST](http://www.fujitsu.com/fts/products/computing/servers/mission-critical/) produtos de servidor. O [pacote de gerenciamento do Fujitsu ServerView](http://www.fujitsu.com/fts/products/computing/servers/primergy/management/) fornece um conjunto de ferramentas abrangente para o servidor de gerenciamento de ciclo de vida, incluindo um agente do lado do servidor que fornece uma interface CIM e o PowerShell para gerenciamento de hardware.

Fujitsu viu uma oportunidade de se integrar facilmente com o Windows Admin Center que os fornecidos interfaces CIM e o PowerShell capaz de se comunicar com os agentes do lado do servidor. A equipe de desenvolvimento em Fujitsu foi capaz de implementar com facilidade as chamadas CIM, que eles estavam familiarizados com para o agente e visualizar as informações dentro do Windows Admin Center usando os componentes de interface do usuário disponíveis.

![Extensão do Fujitsu - modo de exibição de árvore de integridade](../../media/extend-case-study-fujitsu/health-tree.png)

Depois que a equipe se familiarizou com o SDK do Windows Admin Center, adicionando a interface do usuário para expor informações adicionais de hardware geralmente era mais apenas algumas linhas de código HTML e elas conseguiram rapidamente expandir em uma única ferramenta para exibir uma exibição resumida de componente de hardware integridade, as exibições detalhadas para logs de eventos do sistema, o monitor de driver, separe os modos de exibição para o processador, memória, ventiladores, fontes de alimentação, as temperaturas e voltagens e até mesmo uma ferramenta adicional para gerenciamento de RAID. Usando controles de interface do usuário disponíveis no SDK, como a árvore, controles de painel de grade e de detalhes habilitada a equipe a criar rapidamente a interface do usuário e também obter um design visual e interação muito semelhante para o restante do Windows Admin Center.

![Extensão do Fujitsu - modo de exibição de árvore RAID](../../media/extend-case-study-fujitsu/raid-tree.png)

![Extensão do Fujitsu - exibir de volumes RAID](../../media/extend-case-study-fujitsu/raid-volumes.png)

A parceria entre a Fujitsu e a equipe do Windows Admin Center mostra claramente o valor de integração de dentro do Windows Admin Center, permitindo que os clientes tenham informações de ponta a ponta em funções de servidor e serviços, para o sistema operacional e ao gerenciamento de hardware .