---
title: acrescentar
description: Artigo de referência para o comando Append, que permite que os programas abram arquivos de dados em diretórios especificados, como se estivessem no diretório atual.
ms.topic: reference
ms.assetid: 9c3fea20-9502-40ad-a442-7a927aad88eb
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 0002adabaa8c9cbd2235383d997c77670d33d522
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633473"
---
# <a name="append"></a>acrescentar

Permite que os programas abram arquivos de dados em diretórios especificados como se estivessem no diretório atual. Se usado sem parâmetros, **Append** exibe a lista de diretórios acrescentados.

> [!NOTE]
> Este comando não tem suporte no Windows 10.

## <a name="syntax"></a>Sintaxe

```
append [[<drive>:]<path>[;...]] [/x[:on|:off]] [/path:[:on|:off] [/e]
append ;
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `[\<drive>:]<path>` | Especifica uma unidade e um diretório a serem acrescentados. |
| /x: ativado | Aplica diretórios anexados a pesquisas de arquivo e inicialização de aplicativos. |
| /x: off | Aplica diretórios anexados somente a solicitações para abrir arquivos. A opção **/x: off** é a configuração padrão. |
| /Path: on | Aplica diretórios anexados a solicitações de arquivo que já especificam um caminho. **/Path: on** é a configuração padrão. |
| /Path: off | Desativa o efeito de **/Path: on**. |
| /e | Armazena uma cópia da lista de diretórios acrescentados em uma variável de ambiente chamada APPEND. **/e** pode ser usado somente na primeira vez que você usar **Append** depois de iniciar o sistema. |
| ; | Limpa a lista de diretórios acrescentados. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para limpar a lista de diretórios acrescentados, digite:

```
append ;
```

Para armazenar uma cópia do diretório anexado em uma variável de ambiente chamada *Append*, digite:

```
append /e
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
