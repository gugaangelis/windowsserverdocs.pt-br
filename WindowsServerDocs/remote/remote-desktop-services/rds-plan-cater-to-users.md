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
ms.date: 09/23/2016
manager: scottman
ms.openlocfilehash: c04909e9e0cfbf71b6632c154ac8b9b20b5bac10
ms.sourcegitcommit: b4b0e73ce35f8b594eb467a2bb2aa91bd6d86369
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2019
ms.locfileid: "72893084"
---
# <a name="remote-desktop-services---cater-to-different-kinds-of-users"></a>Serviços de Área de Trabalho Remota: atenda a tipos diferentes de usuários

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

Os Serviços de Área de Trabalho Remota têm suporte de diferentes tipos de cargas de trabalho. Escale sua implantação dependendo da necessidade esperada de cada tipo de usuário.

## <a name="types-of-users"></a>Tipos de usuários

### <a name="knowledge-user"></a>Usuário de conhecimento

Os usuários de conhecimento usam aplicativos de produtividade leves como o Microsoft Word, o Excel, o Outlook e o navegador Microsoft Edge.

### <a name="professional-user"></a>Usuário profissional

Os usuários profissionais usam navegadores da Internet e aplicativos de produtividade, além de dar suporte a cargas de trabalho mais intensivas, como o desenvolvimento de software e a criação de conteúdo multimídia.

### <a name="power-user"></a>Usuário avançado

Os usuários avançados usam aplicativos de engenharia e gráficos como o CAD (design auxiliado por computador) e o Adobe Photoshop. As GPUs são muitas vezes uma boa opção para usuários que usam regularmente programas com uso intensivo de gráficos para renderização de vídeo, design 3D e simulações.

Para saber mais sobre a aceleração de gráficos, confira [Escolher a tecnologia de renderização de elementos gráficos](rds-graphics-virtualization.md).

O Azure tem outras opções de implantação de aceleração de gráficos e vários tamanhos de VM de GPU disponíveis. Saiba mais sobre isso em [Tamanhos de máquinas virtuais otimizadas para GPU](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu).

## <a name="test-workload"></a>Carga de trabalho de testes

É recomendável fazer testes de carga das implantações com testes de estresse e simulações de uso da vida real. É possível usar ferramentas de simulação como o LoginVSI para fazer o teste de carga da implantação. Verifique se o sistema é responsivo e resiliente o suficiente para atender às necessidades do usuários e lembre-se de variar o tamanho da carga para evitar surpresas.
