---
title: Ajuste de desempenho para Espaços de Armazenamento Diretos
description: Os Espaços de Armazenamento Diretos ajustam automaticamente o próprio desempenho com base na configuração de cache do hardware que você usar, conforme descrito neste tópico.
ms.topic: article
ms.assetid: 15a519fa-37cc-4d84-a9fe-097d33bb71ea
author: phstee
ms.author: vshankar; danlo; clausjor; stevenek
ms.date: 4/14/2017
ms.openlocfilehash: 18dca2080a311a337e0d41055e95e7a7f0f42a80
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87991983"
---
# <a name="performance-tuning-for-storage-spaces-direct"></a>Ajuste de desempenho para Espaços de Armazenamento Diretos

Espaços de Armazenamento Diretos, uma solução de armazenamento definida por software com base no Windows Server, ajusta automaticamente o próprio desempenho, dispensando a necessidade de especificar manualmente as contagens de coluna, a configuração de cache de hardware que você usar e outros fatores que devem ser definidos manualmente com soluções de armazenamento SAS compartilhadas. Para saber mais, confira [Espaços de Armazenamento Diretos no Windows Server 2016](../../../../storage/storage-spaces/storage-spaces-direct-overview.md).

O Cache de Barramento de Armazenamento de Software dos Espaços de Armazenamento Diretos é configurado automaticamente com base nos tipos de armazenamento presentes no sistema. Três tipos reconhecidos: **HDD**, **SSD** e **NVMe**. O cache tem o armazenamento mais rápido para leitura e/ou gravação em cache, conforme apropriada, e usa o armazenamento mais lento para armazenamento persistente de dados.

A tabela a seguir resume os padrões:

| Tipos de armazenamento | Configuração de cache |
| --- | --- |
| Qualquer tipo único | Se houver apenas um tipo de armazenamento presente, o Cache de Barramento de Armazenamento de Software não estará configurado. |
| SSD + HDD ou NVMe + HDD | O armazenamento mais rápido é configurado como a camada de cache e armazena em cache leituras e gravações. |
| SSD+SSD ou NVMe+NVMe | Essas opções rápido+rápido são direcionadas a combinações de armazenamento de durabilidade maior e menor, por exemplo, 10 gravações de unidade por dia de SSD flash NAND (DWPD) para cache e 1,5 SSD flash NAND 1,5 DWPD para capacidade. Elas são habilitadas ao fornecer aos Espaços de Armazenamento Diretos um conjunto de cadeias de caracteres de Modelo para identificação dos dispositivos de cache. Para saber mais, consulte a referência do cmdlet [Enable-StorageSpacesDirect](https://technet.microsoft.com/library/mt589697.aspx) (`CacheDeviceModel`). <br><br>Em um sistema rápido+rápido, apenas as gravações são armazenadas em cache. Leituras não são armazenadas em cache. |

Observe que o armazenamento em cache em um dispositivo SSD ou NVMe assume o padrão apenas de cache de gravação. A intenção é que, como o dispositivo de capacidade é rápido, há pouco valor na movimentação de conteúdo de leitura para os dispositivos de cache. Há casos em que talvez isso não seja válido, embora seja necessário ter cuidado, pois a habilitação do cache de leitura pode consumir desnecessariamente a durabilidade do dispositivo de cache em aumentar o desempenho. Entre os exemplos estão:

* **NVme+SSD** A habilitação do cache de leitura permitirá a leitura de E/S para tirar proveito da conectividade de PCIe e/ou melhor desempenho de IOPS dos dispositivos NVMe em comparação com o SSD agregado. <br>_Talvez_ isso seja válido para cenários orientados por largura de banda devido aos recursos de largura de banda relativa dos dispositivos NVMe versus o HBA conectando-se ao SSD. _Talvez não_ seja válido para cenários orientados por IOPS nos quais os custos de CPU de IOPS possam limitar os sistemas antes que o aumento do desempenho possa ser obtido.
* **NVMe + NVMe** Da mesma forma, se a capacidade de leitura do cache NVMe for maior do que a capacidade combinada de NVMe, talvez a habilitação do cache de leitura seja válida. <br>Não é comum ter bons casos para o cache de leitura nessas configurações.

Para exibir e alterar a configuração de cache, use os cmdlets [Get-ClusterStorageSpacesDirect](https://technet.microsoft.com/library/mt634616.aspx) e [Set-ClusterStorageSpacesDirect](https://technet.microsoft.com/library/mt763265.aspx). As propriedades `CacheModeHDD` e `CacheModeSSD` definem como o cache opera na mídia de capacidade do tipo indicado.

## <a name="additional-references"></a>Referências adicionais

- [Noções básicas de Espaços de Armazenamento Diretos](../../../../storage/storage-spaces/understand-the-cache.md)
- [Planejar Espaços de Armazenamento Diretos](../../../../storage/storage-spaces/storage-spaces-direct-hardware-requirements.md)
- [Ajuste de desempenho para servidores de arquivo](../../role/file-server/index.md)
- [Guia de considerações de design de armazenamento definido pelo software](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/mt243829(v=ws.11)) (Windows Server 2012 R2 e armazenamento SAS compartilhado)