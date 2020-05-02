---
title: delete volume
description: Tópico de referência para excluir o volume, que exclui o volume selecionado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f625933d-0f47-409e-93b2-a3e234049a5d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b9a8ae0fc863cec5c1a3f6debccf8201e96badd0
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716691"
---
# <a name="delete-volume"></a>delete volume

Exclui o volume selecionado.

## <a name="syntax"></a>Sintaxe

```
delete volume [noerr]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| NOERR | Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |

## <a name="remarks"></a>Comentários

-   Não é possível excluir o volume do sistema, o volume de inicialização ou qualquer volume contendo o arquivo de paginação ativo ou o despejo (despejo de memória).
-   Um volume deve ser selecionado para que essa operação seja realizada com sucesso. Use o comando **selecionar volume** para selecionar um volume e deslocar o foco para ele.

## <a name="examples"></a>Exemplos

Para excluir o volume com foco, digite:
```
delete volume
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

