---
title: Cache de leitura na memória Espaços de Armazenamento Diretos
ms.author: eldenc
manager: siroy
ms.topic: article
author: eldenchristensen
ms.date: 09/21/2020
ms.localizationpriority: medium
ms.openlocfilehash: 94f541b38e0084ae3284dc0e56b2643f23b15bfe
ms.sourcegitcommit: 8a826e992f28a70e75137f876a5d5e61238a24e4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91365329"
---
# <a name="using-storage-spaces-direct-with-the-csv-in-memory-read-cache"></a>Usando Espaços de Armazenamento Diretos com o cache de leitura na memória CSV

> Aplica-se a: Windows Server 2019, Windows Server 2016

Este tópico descreve como usar a memória do sistema para aumentar o desempenho de [espaços de armazenamento diretos](storage-spaces-direct-overview.md).

Espaços de Armazenamento Diretos é compatível com o cache de leitura na memória Volume Compartilhado Clusterizado (CSV). O uso da memória do sistema para leituras de cache pode melhorar O desempenho de aplicativos como o Hyper-V, que usa e/s sem buffer para acessar arquivos VHD ou VHDX. (IOs sem buffer são operações que não são armazenadas em cache pelo Gerenciador de cache do Windows.)

Como o cache na memória é servidor local, ele melhora a localidade de dados para implantações de Espaços de Armazenamento Diretos hiperconvergentes: as leituras recentes são armazenadas em cache na memória no mesmo host em que a máquina virtual está em execução, reduzindo a frequência com que as leituras passam pela rede. Isso resulta em menor latência e melhor desempenho de armazenamento.

## <a name="planning-considerations"></a>Considerações sobre o planejamento

O cache de leitura na memória é mais eficaz para cargas de trabalho com uso intensivo de leitura, como o Virtual Desktop Infrastructure (VDI). Por outro lado, se a carga de trabalho for extremamente de gravação intensa, o cache poderá introduzir mais sobrecarga que o valor e deverá ser desabilitado.

Você pode usar até 80% da memória física total para o cache de leitura na memória CSV.

  > [!TIP]
  > Para implantações hiperconvergentes, em que a computação e o armazenamento são executados nos mesmos servidores, tenha cuidado para deixar memória suficiente para suas máquinas virtuais. Para implantações de Servidor de Arquivos de Escalabilidade Horizontal convergidas (SoFS), com menos contenção de memória, isso não se aplica.

  > [!NOTE]
  > Determinadas ferramentas de MicroBenchMark como DISKSPD e a [VM frota](https://github.com/Microsoft/diskspd/tree/master/Frameworks/VMFleet) podem produzir resultados piores com o cache de leitura em memória CSV habilitado do que sem ele. Por padrão, a VM frota cria 1 10 Gibibyte (GiB) VHDX por máquina virtual – aproximadamente 1 TiB total para as VMs de 100 – e, em seguida, executa leituras e gravações de forma *uniforme aleatórias* . Ao contrário das cargas de trabalho reais, as leituras não seguem nenhum padrão previsível ou repetitivo, portanto, o cache na memória não é eficaz e simplesmente incorre em sobrecarga.

## <a name="configuring-the-in-memory-read-cache"></a>Configurando o cache de leitura na memória

O cache de leitura na memória CSV está disponível no Windows Server 2019 e no Windows Server 2016 com a mesma funcionalidade. No Windows Server 2019, ele é ativado por padrão com 1 GiB alocado. No Windows Server 2016, ele está desativado por padrão.

| Versão do SO             | Tamanho do cache CSV padrão           |
|------------------------|----------------------------------|
| Windows Server 2019    | 1 GiB                            |
| Windows Server 2016    | 0 (desabilitado)                     |
| Windows Server 2012 R2 | habilitado-o usuário deve especificar o tamanho |
| Windows Server 2012    | 0 (desabilitado)                     |

Para ver a quantidade de memória alocada usando o PowerShell, execute:

```PowerShell
(Get-Cluster).BlockCacheSize
```

O valor retornado está em mebibytes (MiB) por servidor. Por exemplo, `1024` representa 1 Gibibyte (GIB).

Para alterar a quantidade de memória alocada, modifique esse valor usando o PowerShell. Por exemplo, para alocar 2 GiB por servidor, execute:

```PowerShell
(Get-Cluster).BlockCacheSize = 2048
```

Para que as alterações entrem em vigor imediatamente, pause e retome os volumes CSV, ou mova-os entre servidores. Por exemplo, use este fragmento do PowerShell para mover cada CSV para outro nó de servidor e voltar novamente:

```PowerShell
Get-ClusterSharedVolume | ForEach {
    $Owner = $_.OwnerNode
    $_ | Move-ClusterSharedVolume
    $_ | Move-ClusterSharedVolume -Node $Owner
}
```

## <a name="additional-references"></a>Referências adicionais

- [Visão geral de Espaços de Armazenamento Diretos](storage-spaces-direct-overview.md)
