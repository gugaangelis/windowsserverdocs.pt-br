---
title: fsutil tiering
description: Tópico de referência para o comando de camadas fsutil, que permite o gerenciamento de funções de camada de armazenamento, como a configuração e a desabilitação de sinalizadores e a listagem de camadas.
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: e5f55f3e-8d2a-4526-8d67-36a539126c22
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 6fa8646dcdf7e836ccb45f253e0c4f8691b1ea3c
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436841"
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
fsutil tiering clearflags C:
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
