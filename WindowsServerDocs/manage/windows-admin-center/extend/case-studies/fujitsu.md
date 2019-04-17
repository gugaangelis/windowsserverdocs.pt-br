---
title: Estudo de caso do Centro de administração do Windows SDK - Fujitsu
description: Estudo de caso do Centro de administração do Windows SDK - Fujitsu
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 05/23/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 6d916920b187dd3c637644a0f40ae9f9cca72b66
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2018
ms.locfileid: "2052370"
---
# <a name="fujitsu-serverview-health-and-raid-extensions"></a>Extensões Fujitsu ServerView da integridade e RAID

## <a name="bringing-end-to-end-visibility-from-operating-system-to-hardware-into-windows-admin-center"></a>Trazendo a visibilidade de ponta a ponta do sistema operacional para hardware, para o Centro de administração do Windows

Fujitsu é um líder informações japonês e empresa de tecnologia de comunicação e um fabricante dos produtos de servidor [PRIMERGY](http://www.fujitsu.com/fts/products/computing/servers/primergy/) e [PRIMEQUEST](http://www.fujitsu.com/fts/products/computing/servers/mission-critical/) . O [pacote de gerenciamento do ServerView de Fujitsu](http://www.fujitsu.com/fts/products/computing/servers/primergy/management/) fornece um conjunto de ferramentas abrangente para servidor de gerenciamento de ciclo de vida, incluindo um agente do lado do servidor que fornece uma interface CIM e PowerShell para gerenciamento de hardware.

Fujitsu através de uma oportunidade de integrar facilmente o Centro de administração do Windows conforme ele fornecido interfaces CIM e PowerShell capaz de se comunicar com os agentes do lado do servidor. A equipe de desenvolvimento da Fujitsu conseguiu facilmente implementar as chamadas CIM fossem familiarizados com para o operador e visualizar as informações no Centro de administração do Windows usando os componentes de interface do usuário disponíveis.

![Extensão Fujitsu - modo de exibição de árvore de integridade](../../media/extend-case-study-fujitsu/health-tree.png)

Depois que a equipe se tornou familiarizada com o Admin Center SDK do Windows, adicionando a interface do usuário para expor informações adicionais de hardware geralmente era mais simplesmente algumas linhas de código HTML e fossem rapidamente podem expandir a partir de uma única ferramenta para mostrar uma exibição de resumida de componente de hardware integridade, visualizações detalhadas de logs de eventos do sistema, monitor de driver, separe-os modos de exibição para o processador, memória, ventilador, fontes de alimentação, temperatura e voltagens e até mesmo a ferramenta adicional para gerenciamento de RAID. Usando controles de interface do usuário disponível no SDK do, como a árvore, controles de painel de grade e detalhes habilitado a equipe criar rapidamente a interface do usuário e também atingir um design visual e interação muito similar ao restante do Centro de administração do Windows.

![Extensão Fujitsu - modo de exibição de árvore RAID](../../media/extend-case-study-fujitsu/raid-tree.png)

![Volumes RAID de extensão Fujitsu - exibir](../../media/extend-case-study-fujitsu/raid-volumes.png)

A parceria entre Fujitsu e a equipe de centro de administração do Windows claramente mostra o valor de integração no Centro de administração do Windows, permitindo que os clientes têm de ponta a ponta percepção funções de servidor e serviços, ao sistema operacional e ao gerenciamento de hardware .