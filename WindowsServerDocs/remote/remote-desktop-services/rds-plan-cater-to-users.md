---
title: 'Serviços de Área de Trabalho Remota: atenda a tipos diferentes de usuários'
description: Descreve os diferentes tipos de usuários para RDS.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da522a18-c33f-468e-b9d6-3ad7d3cfba26
author: spatnaik
ms.author: spatnaik
ms.date: 11/06/2019
manager: scottman
ms.openlocfilehash: dc83d76dbfaded9dadae7c16ae9e1c8df7958a7d
ms.sourcegitcommit: d25e47a54a120b9bc26ff43597518fca58c1cc5b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/07/2019
ms.locfileid: "73753823"
---
# <a name="remote-desktop-services---cater-to-different-kinds-of-users"></a>Serviços de Área de Trabalho Remota: atenda a tipos diferentes de usuários

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

Os Serviços de Área de Trabalho Remota têm suporte de diferentes tipos de cargas de trabalho. Escale sua implantação dependendo da necessidade esperada de cada tipo de usuário.

## <a name="types-of-users"></a>Tipos de usuários

### <a name="knowledge-user"></a>Usuário de conhecimento

Os usuários de conhecimento usam aplicativos de produtividade leves como o Microsoft Word, o Excel, o Outlook e o navegador Microsoft Edge.

Para usuários de conhecimento, recomendamos até quatro usuários por vCPU (CPU virtual).

### <a name="professional-user"></a>Usuário profissional

Os usuários profissionais usam navegadores da Internet e aplicativos de produtividade, além de dar suporte a cargas de trabalho mais intensivas, como o desenvolvimento de software e a criação de conteúdo multimídia.

Para usuários profissionais, recomendamos até dois usuários por vCPU.

### <a name="power-user"></a>Usuário avançado

Os usuários avançados usam aplicativos de engenharia e gráficos como o CAD (design auxiliado por computador) e o Adobe Photoshop. As GPUs são muitas vezes uma boa opção para usuários que usam regularmente programas com uso intensivo de gráficos para renderização de vídeo, design 3D e simulações.

Para usuários avançados, recomendamos apenas um usuário por vCPU.

Para saber mais sobre a aceleração de gráficos, confira [Escolher a tecnologia de renderização de elementos gráficos](rds-graphics-virtualization.md).

O Azure tem outras opções de implantação de aceleração de gráficos e vários tamanhos de VM de GPU disponíveis. Saiba mais sobre isso em [Tamanhos de máquinas virtuais otimizadas para GPU](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu).

## <a name="test-workload"></a>Carga de trabalho de testes

É recomendável fazer testes de carga das implantações com testes de estresse e simulações de uso da vida real. É possível usar ferramentas de simulação para realizar o teste de carga da implantação. Verifique se o sistema é responsivo e resiliente o suficiente para atender às necessidades do usuários e lembre-se de variar o tamanho da carga para evitar surpresas.
