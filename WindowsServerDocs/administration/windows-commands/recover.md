---
title: recover
description: Artigo de referência para o comando de recuperação, que recupera informações legíveis de um disco defeituoso ou defeituoso.
ms.topic: reference
ms.assetid: cf9be2e3-90c8-4773-a201-dc503b91948e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9bb402c5821ae8caa0a83d132a7fbeef9c88b4bd
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89637306"
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
