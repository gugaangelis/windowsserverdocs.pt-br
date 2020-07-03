---
title: recover
description: Artigo de referência para o comando de recuperação, que recupera informações legíveis de um disco defeituoso ou defeituoso.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cf9be2e3-90c8-4773-a201-dc503b91948e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ab7f502b046bf30a40b1fdd386c7faddc5c8f15a
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931931"
---
# <a name="recover"></a>recover

Recupera informações legíveis de um disco defeituoso ou defeituoso. Esse comando lê um arquivo, setor por setor e recupera dados de setores bons. Os dados em setores inválidos são perdidos. Como todos os dados em setores inválidos são perdidos quando você recupera um arquivo, você deve recuperar apenas um arquivo de cada vez.

Os setores inválidos relatados pelo comando **chkdsk** foram marcados como ruins quando o disco estava preparado para operação. Eles não apresentam nenhum perigo e a **recuperação** não os afeta.

## <a name="syntax"></a>Sintaxe

```
recover [<drive>:][<path>]<filename>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `[<drive>:][<path>]<filename>` | Especifica o nome do arquivo (e o local do arquivo se ele não estiver no diretório atual) que você deseja recuperar. O *nome de arquivo* é obrigatório e não há suporte para caracteres curinga. |
| /? | Exibe a ajuda no prompt de comando. |

### <a name="examples"></a>Exemplos

Para recuperar o arquivo *story.txt* no diretório *\Fiction* na unidade D, digite:

```
recover d:\fiction\story.txt
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
