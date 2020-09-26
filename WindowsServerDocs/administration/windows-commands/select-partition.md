---
title: select partition
description: Artigo de referência para o comando selecionar partição, que seleciona a partição especificada e desloca o foco para ela.
ms.topic: reference
ms.assetid: 25f70083-b8f7-4a8e-9b34-4b3ffbe06670
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6418ff1a08bdc12355d2b2dc75bbb7151539ae02
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389229"
---
# <a name="select-partition"></a>select partition

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Seleciona a partição especificada e desloca o foco para ela. Esse comando também pode ser usado para exibir a partição que atualmente tem o foco no disco selecionado.

## <a name="syntax"></a>Sintaxe

```
select partition=<n>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| partição =`<n>` | O número da partição que receberá o foco. Você pode exibir os números de todas as partições no disco selecionado no momento usando o comando **listar partição** no DiskPart. |

#### <a name="remarks"></a>Comentários

- Para poder selecionar uma partição, primeiro você deve selecionar um disco usando o comando **selecionar disco** .

  - Se nenhum número de partição for especificado, essa opção exibirá a partição que atualmente tem o foco no disco selecionado.

  - Se um volume for selecionado com uma partição correspondente, a partição será selecionada automaticamente.

  - Se uma partição for selecionada com um volume correspondente, o volume será selecionado automaticamente.

## <a name="examples"></a>Exemplos

Para deslocar o foco para a *partição 3*, digite:

```
select partitition=3
```

Para exibir a partição que atualmente tem o foco no disco selecionado, digite:

```
select partition
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Create Partition EFI](create-partition-efi.md)

- [comando criar partição estendida](create-partition-extended.md)

- [comando de criação de partição lógica](create-partition-logical.md)

- [comando criar partição MSR](create-partition-msr.md)

- [comando criar partição primária](create-partition-primary.md)

- [comando Excluir partição](delete-partition.md)

- [comando de partição de detalhes](detail-partition.md)

- [Selecionar comando de disco](select-disk.md)

- [Selecionar comando vdisk](select-vdisk.md)

- [Selecionar comando de volume](select-volume.md)
