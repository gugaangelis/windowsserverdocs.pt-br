---
title: select volume
description: Artigo de referência do comando SELECT volume, que seleciona o volume especificado e desloca o foco para ele.
ms.topic: reference
ms.assetid: 5d70d776-80ad-4f20-8288-a7997fb1df28
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 643db6484842e8a67462b0460a1ecdeac653e577
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389184"
---
# <a name="select-volume"></a>select volume

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Seleciona o volume especificado e desloca o foco para ele. Esse comando também pode ser usado para exibir o volume que atualmente tem o foco no disco selecionado.

## <a name="syntax"></a>Sintaxe

```
select volume={<n>|<d>}
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<n>` | O número do volume para receber o foco. Você pode exibir os números de todos os volumes no disco selecionados no momento usando o comando **list volume** no DiskPart. |
| `<d> `| A letra da unidade ou o caminho do ponto de montagem do volume para receber o foco. |

## <a name="remarks"></a>Comentários

- Se nenhum volume for especificado, este comando exibirá o volume que atualmente tem o foco no disco selecionado.

- Em um disco básico, a seleção de um volume também fornece o foco para a partição correspondente.

  - Se um volume for selecionado com uma partição correspondente, a partição será selecionada automaticamente.

  - Se uma partição for selecionada com um volume correspondente, o volume será selecionado automaticamente.

## <a name="examples"></a>Exemplos

Para deslocar o foco para o *volume 2*, digite:

```
select volume=2
```

Para deslocar o foco para a *unidade C*, digite:

```
select volume=c
```

Para deslocar o foco para o volume montado em uma pasta chamada *c:\mountpath*, digite:

```
select volume=c:\mountpath
```

Para exibir o volume que atualmente tem o foco no disco selecionado, digite:

```
select volume
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Adicionar comando de volume](add-volume.md)

- [comando de volume de atributos](attributes-volume.md)

- [comando criar espelho de volume](create-volume-mirror.md)

- [comando criar RAID de volume](create-volume-raid.md)

- [comando Criar volume simples](create-volume-simple.md)

- [comando Create volume Stripe](create-volume-stripe.md)

- [comando excluir volume](delete-volume.md)

- [comando de volume de detalhes](detail-volume.md)

- [comando de volume fsutil](fsutil-volume.md)

- [comando listar volume](list-volume.md)

- [comando de volume offline](offline-volume.md)

- [comando de volume onine](online-volume.md)

- [Selecionar comando de disco](select-disk.md)

- [Selecionar comando de partição](select-partition.md)

- [Selecionar comando vdisk](select-vdisk.md)
