---
title: Cache de leitura de armazenamento direto de espaços de memória
ms.prod: windows-server-threshold
ms.author: eldenc
ms.manager: siroy
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 02/20/2019
ms.localizationpriority: medium
ms.openlocfilehash: 7ed5894a569d4c42a3a4b0e018de5171f2c84a62
ms.sourcegitcommit: 5ed023a2ef3a9002daf41c7717feb1df186d2a14
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/02/2019
ms.locfileid: "9122049"
---
# Usando espaços de armazenamento diretos com CSV na memória cache de leitura
> Aplicável a: Windows Server 2016, Windows Server 2019

Este tópico descreve como usar a memória do sistema para melhorar o desempenho dos [Espaços de armazenamento diretos](storage-spaces-direct-overview.md).

Espaços de armazenamento diretos é compatível com a Volume compartilhado clusterizado (CSV) na memória cache de leitura. Usando a memória do sistema para leituras de cache pode melhorar o desempenho para aplicativos como o Hyper-V, que usa a e/s sem buffer para acessar os arquivos VHD ou VHDX. (Sem buffer IOs são operações que não são armazenadas em cache pelo Gerenciador de Cache do Windows).

Como o cache de memória é o local do servidor, ele melhora a localidade de dados para implantações de espaços de armazenamento diretos hiperconvergentes: Leituras recentes são armazenados em cache na memória no mesmo host em que a máquina virtual está em execução, reduzindo a frequência leituras passar pela rede. Isso resulta em menor latência e melhor desempenho de armazenamento.

## Considerações de planejamento

A leitura na memória cache é mais eficiente para leitura com uso intenso de cargas de trabalho, como Virtual Desktop Infrastructure (VDI). Por outro lado, se a carga de trabalho é extremamente intensivo de gravação, o cache pode introduzir mais sobrecarga do que o valor e deve ser desabilitado.

Você pode usar até 80% do total de memória física para o cache de leitura CSV na memória.

  > [!TIP]
  > Para implantações hiperconvergidas, onde de computação e armazenamento executar nos mesmos servidores, tenha cuidado para deixar memória suficiente para suas máquinas virtuais. Para implantações de servidor de arquivos de escalabilidade horizontal (SoFS) convergidas, com menos contenção de memória, isso não se aplica.

  > [!NOTE]
  > Determinadas ferramentas microbenchmarking como DISKSPD e [Frota VM](https://github.com/Microsoft/diskspd/tree/master/Frameworks/VMFleet) podem pior produzir resultados com CSV na memória cache de leitura habilitado que sem ele. Por padrão a VM frota cria um Chaveta 10 VHDX por máquina virtual – aproximadamente 1 TiB total para 100 VMs – e, em seguida, executa leituras *aleatórias uniformemente* e grava-los. Ao contrário das cargas de trabalho reais, as leituras não siga qualquer padrão repetido ou previsível, portanto, o cache de memória não é eficaz e apenas gera sobrecarga.

## Configurando na memória cache de leitura

Cache de leitura a CSV na memória disponível no Windows Server 2016 e Windows Server 2019 com a mesma funcionalidade. No Windows Server 2016, ela está desativada por padrão. No Windows Server 2019, ele está ativado por padrão com 1 GB alocado.

| Versão do sistema operacional          | Tamanho do cache CSV padrão |
|---------------------|------------------------|
| Windows Server 2016 | 0 (desabilitado)           |
| Windows Server 2019 | 1 Chaveta                   |

Para ver a quantidade de memória alocada usando o PowerShell, execute:

```PowerShell
(Get-Cluster).BlockCacheSize
```

O valor retornado é no mebibytes (MiB) por servidor. Por exemplo, `1024` representa 1 Gibibytes (Chaveta).

Para alterar a quantidade de memória alocada, modificar esse valor usando o PowerShell. Por exemplo, para alocar Chaveta 2 por servidor, execute:

```PowerShell
(Get-Cluster).BlockCacheSize = 2048
```

Para que as alterações entram em vigor imediatamente, pausar e retomar os volumes CSV ou movê-los entre servidores. Por exemplo, use esse fragmento do PowerShell para mover cada CSV para outro nó de servidor e novamente:

```PowerShell
Get-ClusterSharedVolume | ForEach {
    $Owner = $_.OwnerNode
    $_ | Move-ClusterSharedVolume
    $_ | Move-ClusterSharedVolume -Node $Owner
}
```

## Consulte também

- [Visão geral de Espaços de Armazenamento Diretos](storage-spaces-direct-overview.md)
