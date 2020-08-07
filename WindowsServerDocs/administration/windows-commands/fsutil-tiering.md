---
title: fsutil tiering
description: Artigo de referência para o comando de camadas fsutil, que permite o gerenciamento de funções de camada de armazenamento, como a configuração e a desabilitação de sinalizadores e a listagem de camadas.
manager: dmoss
ms.author: toklima
author: toklima
ms.assetid: e5f55f3e-8d2a-4526-8d67-36a539126c22
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 2aa4d82fd5b99bfac508d02628256557e8ed6044
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87889807"
---
# <a name="fsutil-tiering"></a>fsutil tiering

> Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016, Windows 10

Habilita o gerenciamento de funções de camada de armazenamento, como a configuração e a desabilitação de sinalizadores e a listagem de camadas.

## <a name="syntax"></a>Sintaxe

```
fsutil tiering [clearflags] <volume> <flags>
fsutil tiering [queryflags] <volume>
fsutil tiering [regionlist] <volume>
fsutil tiering [setflags] <volume> <flags>
fsutil tiering [tierlist] <volume>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| clearflags | Desabilita os sinalizadores de comportamento de camadas de um volume. |
| `<volume>` | Especifica o volume. |
| /trnh | Para volumes com armazenamento em camadas, o faz com que a coleta de calor seja desabilitada.<p>Aplica-se somente a NTFS e ReFS. |
| queryflags | Consulta os sinalizadores de comportamento de camadas de um volume. |
| regiãolist | Lista as regiões em camadas de um volume e suas respectivas camadas de armazenamento. |
| SetFlags | Habilita os sinalizadores de comportamento de camadas de um volume. |
| camadalist | Lista as camadas de armazenamento associadas a um volume. |

### <a name="examples"></a>Exemplos

Para consultar os sinalizadores no volume C, digite:

```
fsutil tiering queryflags C:
```

Para definir os sinalizadores no volume C, digite:

```
fsutil tiering setflags C: /trnh
```

Para limpar os sinalizadores no volume C, digite:

```
fsutil tiering clearflags C: /trnh
```

Para listar as regiões do volume C e suas respectivas camadas de armazenamento, digite:

```
fsutil tiering regionlist C:
```

Para listar as camadas do volume C, digite:

```
fsutil tiering tierlist C:
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [fsutil](fsutil.md)
