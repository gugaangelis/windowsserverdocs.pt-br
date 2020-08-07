---
title: select volume
description: Artigo de referência para * * * *-
ms.topic: article
ms.assetid: 5d70d776-80ad-4f20-8288-a7997fb1df28
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 394afbc4cb046968d9b1e1d88a272598dc23d3b9
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87882792"
---
# <a name="select-volume"></a>select volume

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

seleciona o volume especificado e desloca o foco para ele. Esse comando também pode ser usado para exibir o volume que atualmente tem o foco no disco selecionado.



## <a name="syntax"></a>Sintaxe

```
select volume={<n>|<d>}
```

### <a name="parameters"></a>Parâmetros

| Parâmetro |                                                                               Descrição                                                                                |
|-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <n>    | O número do volume para receber o foco. Você pode exibir os números de todos os volumes no disco selecionados no momento usando o comando **list volume** no DiskPart. |
|    <d>    |                                                 A letra da unidade ou o caminho do ponto de montagem do volume para receber o foco.                                                 |

## <a name="remarks"></a>Comentários

-   Se nenhum volume for especificado, este comando exibirá o volume que atualmente tem o foco no disco selecionado.

-   Em um disco básico, a seleção de um volume também fornece o foco para a partição correspondente.

-   se um volume for selecionado com uma partição correspondente, a partição será selecionada automaticamente.

-   se uma partição for selecionada com um volume correspondente, o volume será selecionado automaticamente.

## <a name="examples"></a>Exemplos
Para deslocar o foco para o volume 2, digite:

```
select volume=2
```

Para deslocar o foco para a unidade C, digite:

```
select volume=c
```

Para deslocar o foco para o volume montado em uma pasta chamada mountpath, digite:

```
select volume=c:\mountpath
```

Para exibir o volume que atualmente tem o foco no disco selecionado, digite:

```
select volume
```

## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)




