---
title: Cargas de trabalho de Área de Trabalho Remota
description: Uma breve visão geral de diferentes tipos de cargas de trabalho para máquinas virtuais gerenciadas pela Área de Trabalho Remota.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 12/02/2019
ms.topic: article
author: Heidilohr
manager: lizross
ms.openlocfilehash: 53c120be0ca91f0a9331a0585be8b4335f863a56
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857239"
---
# <a name="remote-desktop-workloads"></a>Cargas de trabalho de Área de Trabalho Remota

Os usuários podem executar diferentes tipos de cargas de trabalho nas máquinas virtuais gerenciadas pelo Serviços de Área de Trabalho Remota ou pela Área de Trabalho Virtual do Windows. Escale sua implantação dependendo da necessidade esperada de cada tipo de usuário. A tabela a seguir apresenta exemplos de um intervalo de tipos de carga de trabalho para ajudar a estimar que tamanho as máquinas virtuais devem ter. Depois de configurar suas máquinas virtuais, você deve monitorar continuamente o uso real e ajustar o tamanho de acordo. Se você acabar precisando de uma máquina virtual maior ou menor, poderá facilmente dimensionar sua implantação existente para mais ou para menos no Azure.

A tabela a seguir descreve cada carga de trabalho. Os "usuários de exemplo" são os tipos de usuários que podem considerar cada carga de trabalho mais útil. Os "aplicativos de exemplo" são os tipos de aplicativos que funcionam melhor para cada carga de trabalho.

| Tipo de carga de trabalho | Exemplo de usuários | Aplicativos de exemplo |
| --- | --- | --- |
| Leve | Usuários fazendo tarefas básicas de entrada de dados | Aplicativos de entrada de banco de dados, interfaces de linha de comando |
| Médio | Consultores e pesquisadores de mercado | Aplicativos de entrada de banco de dados, interfaces de linha de comando, Microsoft Word, páginas da Web estáticas |
| Intenso | Engenheiros de software, criadores de conteúdo | Aplicativos de entrada de banco de dados, interfaces de linha de comando, Microsoft Word, páginas da Web estáticas, Microsoft Outlook, Microsoft PowerPoint, páginas da Web dinâmicas |
| Energia | Designers gráficos, criadores de modelos 3D, pesquisadores de aprendizado de máquina | Aplicativos de entrada de banco de dados, interfaces de linha de comando, Microsoft Word, páginas da Web estáticas, Microsoft Outlook, Microsoft PowerPoint, páginas da Web dinâmicas, Adobe Photoshop, Adobe Illustrator, CAD (design auxiliado por computador), CAM (manufatura auxiliada por computador) |

Para obter informações sobre as recomendações de dimensionamento, confira as [Diretrizes de dimensionamento de máquina virtual](virtual-machine-recs.md).
