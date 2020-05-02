---
title: acrescentar
description: Tópico de referência para o comando Append, que permite que os programas abram arquivos de dados em diretórios especificados, como se estivessem no diretório atual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9c3fea20-9502-40ad-a442-7a927aad88eb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 562a13c6b1a47e43bb66548902f0b8e57e789a34
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718994"
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
