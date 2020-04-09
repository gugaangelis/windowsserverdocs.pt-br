---
title: delete volume
description: Tópico de comandos do Windows para excluir volume, que exclui o volume selecionado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f625933d-0f47-409e-93b2-a3e234049a5d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f2e958785c278306563999b09c1fecc0fdfa7ecb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846549"
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
| NOERR | somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |

## <a name="remarks"></a>Comentários

-   Não é possível excluir o volume do sistema, o volume de inicialização ou qualquer volume contendo o arquivo de paginação ativo ou o despejo (despejo de memória).
-   Um volume deve ser selecionado para que essa operação seja realizada com sucesso. Use o comando **selecionar volume** para selecionar um volume e deslocar o foco para ele.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para excluir o volume com foco, digite:
```
delete volume
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

