---
title: Diretrizes de rede
description: Recomendações de largura de banda para implantações da Área de Trabalho Remota.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 12/12/2019
ms.topic: article
author: Heidilohr
manager: lizross
ms.openlocfilehash: 79db56d467ae0913446faebffc5a9598aae0b767
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852989"
---
# <a name="network-guidance"></a>Diretrizes de rede

Ao usar uma sessão remota do Windows, a largura de banda disponível da rede afeta muito a qualidade da sua experiência. Diferentes aplicativos e resoluções de exibição exigem configurações de rede diferentes. Portanto, é importante verificar se a rede está configurada para atender às suas necessidades.

>[!NOTE]
>As recomendações a seguir se aplicam a redes com menos de 0,1% de perda. Essas recomendações se aplicam independentemente de quantas sessões você está hospedando em suas VMs (máquinas virtuais).

## <a name="applications"></a>Aplicativo

A tabela a seguir lista as larguras de banda mínimas recomendadas para uma experiência de usuário tranquila. Essas recomendações se baseiam nas diretrizes da seção [Cargas de trabalho de Área de Trabalho Remota](remote-desktop-workloads.md).

| Tipo de carga de trabalho   | Largura de banda recomendada |
|-----------------|-----------------------|
| Leve           | 1.5 Mbps              |
| Médio          | 3 Mbps                |
| Intenso           | 5 Mbps                |
| Energia           | 15 Mbps               |

Tenha em mente que o estresse colocado em sua rede depende tanto da taxa de quadros de saída da carga de trabalho do aplicativo quanto da sua resolução de vídeo. Se a taxa de quadros ou a resolução de vídeo aumentar, o requisito de largura de banda também aumentará. Por exemplo, uma carga de trabalho leve com uma exibição de alta resolução exige mais largura de banda disponível do que uma carga de trabalho leve com resolução regular ou baixa.

Outros cenários podem ter seus requisitos de largura de banda alterados, dependendo de como você os usa, como:

- Conferência de voz ou vídeo
- Comunicação em tempo real
- Vídeo de 4K em streaming

Certifique-se de realizar o teste de carga desses cenários em sua implantação usando ferramentas de simulação, como o VSI de logon. Varie o tamanho do carregamento, execute testes de estresse e de cenários de usuário comuns em sessões remotas para entender melhor os requisitos da sua rede.

## <a name="display-resolutions"></a>Resoluções de vídeo

Resoluções de vídeo diferentes exigem larguras de banda disponíveis diferentes. A tabela a seguir lista as larguras de banda que recomendamos para uma experiência de usuário tranquila em resoluções de vídeo típicas com uma taxa de quadros de 30 quadros por segundo (fps). Essas recomendações se aplicam a cenários de usuário únicos e múltiplos. Tenha em mente que os cenários que envolvem uma taxa de quadros inferior a 30 fps, como a leitura de texto estático, exigem menos largura de banda disponível.

| Resoluções de vídeo típicas a 30 fps    | Largura de banda recomendada |
|------------------------------------------|-----------------------|
| Cerca de 1.024 × 768 px                      | 1.5 Mbps              |
| Cerca de 1.280 × 720 px                      | 3 Mbps                |
| Cerca de 1.920 × 1.080 px                     | 5 Mbps                |
| Cerca de 3.840 × 2.160 px (4K)                | 15 Mbps               |

## <a name="additional-resources"></a>Recursos adicionais

A região do Azure em que você está pode afetar a experiência do usuário tanto quanto as condições de rede. Confira o [Estimador de experiência da Área de Trabalho Virtual do Windows](https://azure.microsoft.com/services/virtual-desktop/assessment/) para saber mais detalhes.
