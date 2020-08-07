---
title: endlocal
description: Artigo de referência para o comando endlocal, que encerra a localização de alterações de ambiente em um arquivo em lotes e restaura as variáveis de ambiente para seus valores antes da execução do comando SETLOCAL correspondente.
ms.topic: article
ms.assetid: 765fae3c-0c0a-4639-99a4-cf613489b949
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a7b4b95b38fc3b7bd333c131296bf07bbabeb378
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87890603"
---
# <a name="endlocal"></a>endlocal

Encerra a localização de alterações de ambiente em um arquivo em lotes e restaura as variáveis de ambiente para seus valores antes da execução do comando **setlocal** correspondente.

## <a name="syntax"></a>Sintaxe

```
endlocal
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | DESCRIÇÃO |
| --------- | ----------- |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- O comando **ENDLOCAL** não tem nenhum efeito fora de um script ou arquivo em lotes.

- Há um comando **ENDLOCAL** implícito no final de um arquivo em lotes.

- Se as extensões de comando estiverem habilitadas (as extensões de comando são habilitadas por padrão), o comando **ENDLOCAL** restaura o estado das extensões de comando (isto é, habilitado ou desabilitado) para o que era antes de o comando **setlocal** correspondente ser executado.

> [!NOTE]
> Para obter mais informações sobre como habilitar e desabilitar extensões de comando, consulte o [comando cmd](cmd.md).

### <a name="examples"></a>Exemplos

Você pode localizar variáveis de ambiente em um arquivo em lotes. Por exemplo, o programa a seguir inicia o programa *superapp* batch na rede, direciona a saída para um arquivo e exibe o arquivo no bloco de notas:

```
@echo off
setlocal
path=g:\programs\superapp;%path%
call superapp>c:\superapp.out
endlocal
start notepad c:\superapp.out
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
