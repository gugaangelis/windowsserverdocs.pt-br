---
title: append
description: O tópico de comandos do Windows para **Append**, que permite que os programas abram arquivos de dados em diretórios especificados, como se estivessem no diretório atual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9c3fea20-9502-40ad-a442-7a927aad88eb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 95bbc607ef297e7cf67da2e388884882356ef744
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851319"
---
# <a name="append"></a>append

Permite que os programas abram arquivos de dados em diretórios especificados como se estivessem no diretório atual. Se usado sem parâmetros, **Append** exibe a lista de diretórios acrescentados.

> [!NOTE]
> Este comando não tem suporte no Windows 10.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
append [[<Drive>:]<Path>[;...]] [/x[:on|:off]] [/path:[:on|:off] [/e] 
append ;
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `[\<Drive>:]<Path>` | Especifica uma unidade e um diretório a serem acrescentados. |
| `/x:on` | Aplica diretórios anexados a pesquisas de arquivo e inicialização de aplicativos. |
| `/x:off` | Aplica diretórios anexados somente a solicitações para abrir arquivos. A opção **/x: off** é a configuração padrão. |
| `/path:on` | Aplica diretórios anexados a solicitações de arquivo que já especificam um caminho. **/Path: on** é a configuração padrão. |
| `/path:off` | Desativa o efeito de **/Path: on**. |
| `/e` | Armazena uma cópia da lista de diretórios acrescentados em uma variável de ambiente chamada APPEND. **/e** pode ser usado somente na primeira vez que você usar **Append** depois de iniciar o sistema. |
| `;` | Limpa a lista de diretórios acrescentados. |
| `/?` | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para limpar a lista de diretórios acrescentados, digite:

```
append ;
```

Para armazenar uma cópia do diretório anexado em uma variável de ambiente chamada APPEND, digite:

```
append /e
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
