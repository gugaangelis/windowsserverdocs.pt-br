---
title: fsutil wim
description: Artigo de referência para o comando fsutil wim, que fornece funções para descobrir e gerenciar arquivos com suporte para a imagem do Windows (WIM).
manager: dmoss
ms.author: toklima
author: toklima
ms.assetid: 6c6ff819-f349-4aea-b0be-1f637f631736
ms.topic: reference
ms.date: 10/16/2017
ms.openlocfilehash: 6e1bb492911250161e1e4a57983ea9c58f3a34ff
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89032864"
---
# <a name="fsutil-wim"></a>fsutil wim

> Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016, Windows 10

Fornece funções para descobrir e gerenciar arquivos com suporte de imagem do Windows (WIM).

## <a name="syntax"></a>Sintaxe

```
fsutil wim [enumfiles] <drive name> <data source>
fsutil wim [enumwims] <drive name>
fsutil wim [queryfile] <filename>
fsutil wim [removewim] <drive name> <data source>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| enumfiles | Enumera arquivos de backup do WIM. |
| `<drive name>` | Especifica o nome da unidade. |
| `<data source>` | Especifica a fonte de dados. |
| enumwims | Enumera os arquivos WIM de backup. |
| queryfile | Consulta se o arquivo tem o suporte do WIM e, em caso afirmativo, exibe detalhes sobre o arquivo WIM. |
| `<filename>` | Especifica o nome do arquivo. |
| removewim | Remove um WIM do backup de arquivos. |

### <a name="examples"></a>Exemplos

Para enumerar os arquivos da unidade C: da fonte de dados 0, digite:

```
fsutil wim enumfiles C: 0
```

Para enumerar arquivos WIM de backup para a unidade C:, digite:

```
fsutil wim enumwims C:
```

Para ver se um arquivo tem o suporte do WIM, digite:

```
fsutil wim C:\Windows\Notepad.exe
```

Para remover o WIM de arquivos de backup para o volume C: e a fonte de dados 2, digite:

```
fsutil wim removewims C: 2
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [fsutil](fsutil.md)
