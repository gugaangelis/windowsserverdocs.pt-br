---
title: append
description: 'Tópico de comandos do Windows para '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9c3fea20-9502-40ad-a442-7a927aad88eb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fdc4243bee8055888b023a56921cef757dda6b7e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382753"
---
# <a name="append"></a>append



Permite que os programas abram arquivos de dados em diretórios especificados como se estivessem no diretório atual. Se usado sem parâmetros, **Append** exibe a lista de diretórios acrescentados.

> [!NOTE]
> Este comando não tem suporte no Windows 10.
>

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
append [[<Drive>:]<Path>[;...]] [/x[:on|:off]] [/path:[:on|:off] [/e] 
append ;
```

## <a name="parameters"></a>Parâmetros

|     Parâmetro     |                                                                                 Descrição                                                                                 |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [\<Drive >:] <Path> |                                                                 Especifica uma unidade e um diretório a serem acrescentados.                                                                  |
|       /x: ativado       |                                                  Aplica diretórios anexados a pesquisas de arquivo e inicialização de aplicativos.                                                  |
|      /x: off       |                                     Aplica diretórios anexados somente a solicitações para abrir arquivos.</br>**/x: off** é a configuração padrão.                                     |
|     /Path: on      |                               Aplica diretórios anexados a solicitações de arquivo que já especificam um caminho. **/Path: on** é a configuração padrão.                               |
|     /Path: off     |                                                                    Desativa o efeito de **/Path: on**.                                                                    |
|        /e         | Armazena uma cópia da lista de diretórios acrescentados em uma variável de ambiente chamada APPEND. **/e** pode ser usado somente na primeira vez que você usar **Append** depois de iniciar o sistema. |
|         ;         |                                                                     Limpa a lista de diretórios acrescentados.                                                                     |
|        /?         |                                                                    Exibe a ajuda no prompt de comando.                                                                     |

## <a name="BKMK_examples"></a>Disso

Para limpar a lista de diretórios acrescentados, digite:
```
append ;
```
Para armazenar uma cópia do diretório anexado em uma variável de ambiente chamada APPEND, digite:
```
append /e
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
