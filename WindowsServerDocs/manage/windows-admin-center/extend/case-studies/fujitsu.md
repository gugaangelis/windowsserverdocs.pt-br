---
title: Estudo de caso do SDK do centro de administração do Windows-Fujitsu
description: Estudo de caso do SDK do centro de administração do Windows-Fujitsu
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 05/23/2018
ms.localizationpriority: medium
ms.openlocfilehash: 15f0120fdf1792ad127a9f5fa0e045ef7df18e85
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87958744"
---
# <a name="fujitsu-serverview-health-and-raid-extensions"></a>Extensões de integridade e RAID do Fujitsu ServerView

## <a name="bringing-end-to-end-visibility-from-operating-system-to-hardware-into-windows-admin-center"></a>Levando a visibilidade de ponta a ponta, do sistema operacional ao hardware, no centro de administração do Windows

A Fujitsu é uma empresa líder em japonês de tecnologia de informações e comunicação e um fabricante de produtos de servidor [PRIMERGY](http://www.fujitsu.com/fts/products/computing/servers/primergy/) e [PRIMEQUEST](http://www.fujitsu.com/fts/products/computing/servers/mission-critical/) . O [Fujitsu ServerView Management Suite](http://www.fujitsu.com/fts/products/computing/servers/primergy/management/) fornece um conjunto de ferramentas abrangente para o gerenciamento do ciclo de vida do servidor, incluindo um agente do servidor que fornece uma interface CIM e do PowerShell para o gerenciamento de hardware.

A Fujitsu observou uma oportunidade de se integrar facilmente ao centro de administração do Windows, pois forneceu as interfaces CIM e PowerShell que poderiam se comunicar com os agentes do lado do servidor. A equipe de desenvolvimento da Fujitsu conseguiu implementar facilmente as chamadas de CIM com as quais estavam familiarizados com o agente e visualizar as informações no centro de administração do Windows usando os componentes de interface do usuário disponíveis.

![Extensão Fujitsu – exibição de árvore de integridade](../../media/extend-case-study-fujitsu/health-tree.png)

Depois que a equipe ficou familiar com o SDK do centro de administração do Windows, a adição da interface do usuário para expor informações de hardware adicionais geralmente era simplesmente mais algumas linhas de código HTML e elas podiam rapidamente se expandir de uma única ferramenta para exibir uma exibição resumida da integridade do componente de hardware, exibições detalhadas para logs de eventos do sistema, monitor de driver, exibições separadas para processador , ventiladores, fontes de alimentação, temperaturas e voltagens e até mesmo uma ferramenta adicional para o gerenciamento de RAID. O uso de controles de interface do usuário disponíveis no SDK, como os controles de árvore, grade e detalhes do painel, permitiu que a equipe criasse a interface do usuário rapidamente e também obterá um design visual e de interação muito semelhante ao restante do centro de administração do Windows.

![Extensão Fujitsu-exibição de árvore RAID](../../media/extend-case-study-fujitsu/raid-tree.png)

![Extensão Fujitsu-exibição de volumes RAID](../../media/extend-case-study-fujitsu/raid-volumes.png)

A parceria entre a Fujitsu e a equipe do centro de administração do Windows mostra claramente o valor da integração no centro de administração do Windows, permitindo que os clientes tenham informações de ponta a ponta sobre funções e serviços de servidor, para o sistema operacional e para o gerenciamento de hardware.