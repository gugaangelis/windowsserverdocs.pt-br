---
title: create volume raid
description: Artigo de referência para o comando Create volume RAID, que cria um volume RAID-5 usando três ou mais discos dinâmicos especificados.
ms.topic: reference
ms.assetid: 9f257950-9240-4d5f-9537-8ad653d48ebf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c936205f6132a8a2bc06e455dd07761715df684f
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033144"
---
# <a name="create-volume-raid"></a>create volume raid

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria um volume RAID-5 usando três ou mais discos dinâmicos especificados. Depois de criar o volume, o foco mudará automaticamente para o novo volume.

## <a name="syntax"></a>Sintaxe

```
create volume raid [size=<n>] disk=<n>,<n>,<n>[,<n>,...] [align=<n>] [noerr]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| tamanho =`<n>` | A quantidade de espaço em disco, em megabytes (MB), que o volume ocupará em cada disco. Se nenhum tamanho for fornecido, o maior volume RAID-5 possível será criado. O disco com o menor espaço livre contíguo disponível determina o tamanho do volume RAID-5 e a mesma quantidade de espaço é alocada de cada disco. A quantidade real de espaço em disco utilizável no volume RAID-5 é menor do que a quantidade combinada de espaço em disco porque parte do espaço em disco é necessário para a paridade. |
| disco =`<n>,<n>,<n>[,<n>,...]` | Os discos dinâmicos nos quais criar o volume RAID-5. Você precisa de pelo menos três discos dinâmicos para criar um volume RAID-5. Uma quantidade de espaço igual a `size=<n>` é alocada em cada disco. |
| align =`<n>` | Alinha todas as extensões de volume ao limite de alinhamento mais próximo. Normalmente usado com matrizes de LUN (número de unidade lógica) RAID de hardware para melhorar o desempenho. `<n>` é o número de kilobytes (KB) desde o início do disco até o limite de alinhamento mais próximo. |
| NOERR | Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |

## <a name="examples"></a>Exemplos

Para criar um volume RAID-5 de 1000 megabytes de tamanho, usando os discos 1, 2 e 3, digite:

```
create volume raid size=1000 disk=1,2,3
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Create](create.md)
