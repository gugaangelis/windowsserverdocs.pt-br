---
title: Cache de leitura de espaços diretos em-memória de armazenamento
ms.prod: windows-server-threshold
ms.author: eldenc
ms.manager: siroy
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 02/20/2019
ms.localizationpriority: medium
ms.openlocfilehash: 7ed5894a569d4c42a3a4b0e018de5171f2c84a62
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850547"
---
# <a name="using-storage-spaces-direct-with-the-csv-in-memory-read-cache"></a>Usar espaços de armazenamento diretos com o CSV na memória cache de leitura
> Aplica-se a: Windows Server 2016, Windows Server 2019

Este tópico descreve como usar a memória do sistema para melhorar o desempenho do [espaços de armazenamento diretos](storage-spaces-direct-overview.md).

Espaços de armazenamento diretos é compatível com a Cluster CSV (Volume compartilhado) na memória cache de leitura. Usando a memória do sistema para leituras de cache pode melhorar o desempenho de aplicativos como o Hyper-V, que usa e/s não armazenada em buffer para acessar os arquivos VHD ou VHDX. (Sem buffer IOs são todas as operações que não são armazenadas em cache pelo Gerenciador de Cache do Windows).

Porque o cache na memória é o local do servidor, ele melhora a localidade dos dados de implantações de espaços de armazenamento diretos hiperconvergentes: Leituras recentes são armazenadas em cache na memória no mesmo host onde a máquina virtual está em execução, reduzindo a frequência com que as leituras vão pela rede. Isso resulta em uma latência menor e melhor desempenho de armazenamento.

## <a name="planning-considerations"></a>Considerações de planejamento

Cache de leitura na memória mais eficiente para cargas de trabalho de leitura intensa, como o Virtual Desktop Infrastructure (VDI). Por outro lado, se a carga de trabalho for muito intensivo de gravação, o cache pode introduzir mais sobrecarga do que o valor e deve ser desabilitado.

Você pode usar até 80% da memória física total para o CSV na memória cache de leitura.

  > [!TIP]
  > Para implantações hiperconvergentes, em que a computação e armazenamento executado nos mesmos servidores, tenha cuidado para deixar memória suficiente para suas máquinas virtuais. Para implantações de servidor de arquivos de escalabilidade horizontal (SoFS) convergidas, com menor contenção da memória, isso não se aplica.

  > [!NOTE]
  > Determinadas ferramentas de microbenchmarking como DISKSPD e [frota de VM](https://github.com/Microsoft/diskspd/tree/master/Frameworks/VMFleet) pode produzir resultados piores com o CSV na memória cache de leitura habilitado que sem ele. Por padrão, frota de VM cria um VHDX por máquina virtual – aproximadamente 1 TiB de 10 GiB total para 100 VMs – e, em seguida, executa *aleatório uniformemente* lê e grava-os. Ao contrário de cargas de trabalho real, as leituras não seguem qualquer padrão repetitiva ou previsível, para que o cache na memória não é eficaz e apenas incorre em sobrecarga.

## <a name="configuring-the-in-memory-read-cache"></a>Configurando na memória cache de leitura

O CSV na memória ler o cache está disponível no Windows Server 2016 e Windows Server 2019 com a mesma funcionalidade. No Windows Server 2016, ele está desativado por padrão. No Windows Server de 2019, ela é ativada por padrão com 1 GB alocado.

| Versão do SO          | Tamanho padrão do cache CSV |
|---------------------|------------------------|
| Windows Server 2016 | 0 (desabilitado)           |
| Windows Server 2019 | 1 GiB                   |

Para ver a quantidade de memória é alocado usando o PowerShell, execute:

```PowerShell
(Get-Cluster).BlockCacheSize
```

O valor retornado está em mebibytes (MiB) por servidor. Por exemplo, `1024` representa 1 gibibyte (GiB).

Para alterar a quantidade de memória é alocada, modifique esse valor usando o PowerShell. Por exemplo, para alocar 2 GiB por servidor, execute:

```PowerShell
(Get-Cluster).BlockCacheSize = 2048
```

Para que as alterações entrem em vigor imediatamente, pausar e retomar seus volumes CSV ou movê-los entre servidores. Por exemplo, use esse fragmento do PowerShell para mover cada CSV para outro nó de servidor e vice-versa:

```PowerShell
Get-ClusterSharedVolume | ForEach {
    $Owner = $_.OwnerNode
    $_ | Move-ClusterSharedVolume
    $_ | Move-ClusterSharedVolume -Node $Owner
}
```

## <a name="see-also"></a>Consulte também

- [Visão geral direta de espaços de armazenamento](storage-spaces-direct-overview.md)
