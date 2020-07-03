---
title: create volume mirror
description: Artigo de referência para o comando Create volume Mirror, que cria um espelho de volume usando os dois discos dinâmicos especificados.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 48776917-783a-47ff-8da4-1cab77cea34b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 91f718aab181db7d3cbeb0e4255a43f2924ecd33
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85929551"
---
# <a name="create-volume-mirror"></a>create volume mirror

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria um espelho de volume usando os dois discos dinâmicos especificados. Depois que o volume tiver sido criado, o foco mudará automaticamente para o novo volume.

## <a name="syntax"></a>Sintaxe

```
create volume mirror [size=<n>] disk=<n>,<n>[,<n>,...] [align=<n>] [noerr]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| tamanho =`<n>` | Especifica a quantidade de espaço em disco, em megabytes (MB), que o volume ocupará em cada disco. Se nenhum tamanho for fornecido, o novo volume ocupará o espaço livre restante no menor disco e uma quantidade igual de espaço em cada disco subsequente. |
| disco = `<n>` , `<n>` [ `,<n>,...` ] | Especifica os discos dinâmicos nos quais o volume espelho é criado. Você precisa de dois discos dinâmicos para criar um volume de espelho. Uma quantidade de espaço igual ao tamanho especificado com o parâmetro de **tamanho** é alocada em cada disco. |
| align =`<n>` | Alinha todas as extensões de volume ao limite de alinhamento mais próximo. Esse parâmetro é normalmente usado com matrizes de LUN (número de unidade lógica) de RAID de hardware para melhorar o desempenho. `<n>`é o número de kilobytes (KB) desde o início do disco até o limite de alinhamento mais próximo. |
| NOERR | Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um erro. |

## <a name="examples"></a>Exemplos

Para criar um volume espelhado de 1000 megabytes de tamanho, nos discos 1 e 2, digite:

```
create volume mirror size=1000 disk=1,2
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Create](create.md)
