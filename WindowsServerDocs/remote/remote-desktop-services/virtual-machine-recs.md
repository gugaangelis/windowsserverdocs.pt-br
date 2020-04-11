---
title: Dimensionamento de máquina virtual
description: Recomendações de tamanho para cada tipo de carga de trabalho.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 12/02/2019
ms.topic: article
author: Heidilohr
manager: lizross
ms.openlocfilehash: 1d6a7daa3966488c951117b083411587d13be56b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857099"
---
# <a name="virtual-machine-sizing-guidance"></a>Orientação de dimensionamento de máquina virtual

Se você estiver executando sua máquina virtual em Serviços de Área de Trabalho Remota ou Área de Trabalho Virtual do Windows, tipos diferentes de cargas de trabalho exigirão configurações diferentes de VM (máquina virtual) do host de sessão. Para obter a melhor experiência possível, dimensione sua implantação dependendo das necessidades dos usuários.

## <a name="multi-session-recommendations"></a>Recomendações de várias sessões

A tabela a seguir lista o número máximo sugerido de usuários por vCPU (unidade de processamento central virtual) e a configuração de VM mínima para cada carga de trabalho. Essas recomendações se baseiam em [cargas de trabalho da Área de Trabalho Remota](remote-desktop-workloads.md).

| Tipo de carga de trabalho | Máximo de usuários por vCPU | Armazenamento mínimo de vCPU/RAM/SO | Exemplo de instâncias do Azure | Armazenamento mínimo de contêiner de perfil |
| --- | --- | --- | --- | --- |
| Leve | 6 | 2 vCPUs, 8 GB de RAM, 16 GB de armazenamento | D2s_v3, F2s_v2 | 30 GB |
| Médio | 4 | 4 vCPUs, 16 GB de RAM, 32 GB de armazenamento | D4s_v3, F4s_v2 | 30 GB |
| Intenso | 2 | 4 vCPUs, 16 GB de RAM, 32 GB de armazenamento | D4s_v3, F4s_v2 | 30 GB |
| Energia | 1 | 6 vCPUs, 56 GB de RAM, 340 GB de armazenamento | D4s_v3, F4s_v2, NV6 | 30 GB |

## <a name="single-session-recommendations"></a>Recomendações de uma única sessão

Para obter recomendações de dimensionamento de VM para cenários de sessão única, recomendamos pelo menos dois núcleos de CPU física por VM (geralmente quatro vCPUs com hyperthreading). Se você precisar de recomendações de dimensionamento de VM mais específicas para cenários de sessão única, peça aos fornecedores de software específicos para sua carga de trabalho. O dimensionamento de VM para VMs de sessão única provavelmente se alinhará com as diretrizes de dispositivo físico.

## <a name="general-virtual-machine-recommendations"></a>Recomendações gerais de máquina virtual

Para obter os requisitos de VM para executar o sistema operacional, confira [Especificações de computador do Windows 10 e requisitos do sistema](https://www.microsoft.com/windows/windows-10-specifications).

Recomendamos que você use o armazenamento SSD Premium em seu disco do SO para cargas de trabalho de produção que exigem um SLA (contrato de nível de serviço). Para obter mais detalhes, confira o [SLA para máquinas virtuais](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/).

As GPUs (unidades de processamento gráfico) muitas vezes são uma boa opção para usuários que usam regularmente programas com uso intensivo de gráficos para renderização de vídeo, design 3D e simulações. Para saber mais sobre a aceleração de gráficos, confira [Escolher a tecnologia de renderização de elementos gráficos](rds-graphics-virtualization.md). O Azure tem várias opções de implantação de aceleração de gráficos e vários tamanhos de VM de GPU disponíveis. Saiba mais em [tamanhos de máquina virtual otimizadas para GPU](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu).

As [VMs expansíveis da série B](https://docs.microsoft.com/azure/virtual-machines/windows/b-series-burstable) são uma boa opção para os usuários que nem sempre precisam de desempenho máximo da CPU. Para obter mais informações sobre tipos e tamanhos de VM, confira [Tamanhos para máquinas virtuais do Windows no Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sizes) e as informações de preço em [nossa página da série de Máquinas Virtuais](https://azure.microsoft.com/pricing/details/virtual-machines/series/).

## <a name="test-your-workload"></a>Testar sua carga de trabalho

Por fim, recomendamos que você use ferramentas de simulação para testar a implantação com testes de estresse e simulações de uso real. Verifique se o sistema é responsivo e resiliente o suficiente para atender às necessidades do usuários e lembre-se de variar o tamanho da carga para evitar surpresas.
