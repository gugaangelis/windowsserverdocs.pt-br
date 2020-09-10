---
title: bitsadmin getreplyfilename
description: Artigo de referência para o comando Bitsadmin getreplyfilename, que obtém o caminho do arquivo que contém a resposta de upload do servidor para o trabalho.
ms.topic: reference
ms.assetid: 85447184-1732-4816-a365-2e3599551bf8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d3dab8e7f2a50c00c1c5ccb72f4e6dea76df3090
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631712"
---
# <a name="bitsadmin-getreplyfilename"></a>bitsadmin getreplyfilename

Obtém o caminho do arquivo que contém a resposta de carregamento do servidor para o trabalho.

> [!NOTE]
> Esse comando não tem suporte no BITS 1,2 e versões anteriores.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getreplyfilename <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar o nome de arquivo de upload-resposta para o trabalho chamado *myDownloadJob*:

```
bitsadmin /getreplyfilename myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
