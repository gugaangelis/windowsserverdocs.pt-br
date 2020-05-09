---
title: criar partição lógica
description: Tópico de referência do comando lógico criar partição, que cria uma partição lógica em uma partição estendida existente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1f59b79a-d690-4d0e-ad38-40df5a0ce38e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 99b8c837fe5da295087f9b146bf429d91ebc8693
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993274"
---
# <a name="create-partition-logical"></a>criar partição lógica

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria uma partição lógica em uma partição estendida existente. Depois que a partição tiver sido criada, o foco mudará automaticamente para a nova partição.

>[!IMPORTANT]
> Você pode usar esse comando somente em discos MBR (registro mestre de inicialização). Você deve usar o comando [selecionar disco](select-disk.md) para selecionar um disco MBR básico e deslocar o foco para ele.
>
> Você deve criar uma [partição estendida](create-partition-extended.md) antes de poder criar unidades lógicas.

## <a name="syntax"></a>Sintaxe

```
create partition logical [size=<n>] [offset=<n>] [align=<n>] [noerr]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| tamanho =`<n>` | Especifica o tamanho da partição lógica em megabytes (MB), que deve ser menor do que a partição estendida. Se nenhum tamanho for fornecido, a partição continuará até que não haja mais espaço livre na partição estendida. |
| deslocamento =`<n>` | Especifica o deslocamento em kilobytes (KB), no qual a partição é criada. O deslocamento é arredondado para preencher completamente qualquer tamanho de cilindro usado. Se nenhum deslocamento for fornecido, a partição será colocada na primeira extensão de disco grande o suficiente para contê-la. A partição é pelo menos tão longa em bytes quanto o número especificado por **Size =`<n>`**. Se você especificar um tamanho para a partição lógica, ela deverá ser menor do que a partição estendida. |
| align =`<n>` | Alinha todas as extensões de volume ou partição ao limite de alinhamento mais próximo. Normalmente usado com matrizes de LUN (número de unidade lógica) RAID de hardware para melhorar o desempenho. `<n>`é o número de kilobytes (KB) desde o início do disco até o limite de alinhamento mais próximo. |
| NOERR | Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |

#### <a name="remarks"></a>Comentários

- Se os parâmetros de **tamanho** e **deslocamento** não forem especificados, a partição lógica será criada na maior extensão de disco disponível na partição estendida.

## <a name="examples"></a>Exemplos

Para criar uma partição lógica de 1000 megabytes de tamanho, na partição estendida do disco selecionado, digite:

```
create partition logical size=1000
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Create](create.md)

- [select disk](select-disk.md)
