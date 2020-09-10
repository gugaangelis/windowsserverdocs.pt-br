---
title: bitsadmin setreplyfilename
description: Artigo de referência para o comando Bitsadmin setreplyfilename, que especifica o caminho do arquivo que contém a resposta de upload do servidor.
ms.topic: reference
ms.assetid: c26d3342-0533-40b1-a13e-e09678232b25
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e517b3b09e287d4364763b433e3209bf8d4794d6
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630639"
---
# <a name="bitsadmin-setreplyfilename"></a>bitsadmin setreplyfilename

Especifica o caminho do arquivo que contém a resposta de upload do servidor.

> [!NOTE]
> Esse comando não tem suporte no BITS 1,2 e versões anteriores.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /setreplyfilename <job> <file_path>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| file_path | Local para colocar o upload do servidor-responder. |

## <a name="examples"></a>Exemplos

Para definir o caminho do arquivo de nome de recarregamento-resposta do trabalho chamado *myDownloadJob*:

```
bitsadmin /setreplyfilename myDownloadJob c:\upload-reply
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
