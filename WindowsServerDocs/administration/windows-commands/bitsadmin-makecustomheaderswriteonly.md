---
title: bitsadmin makecustomheaderswriteonly
description: Artigo de referência para o comando Bitsadmin makecustomheaderswriteonly, que torna os cabeçalhos HTTP personalizados de um trabalho somente gravação.
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 03/01/2019
ms.openlocfilehash: 0eee6cc2fd8c825f02f7e01750be743b10beb73e
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631487"
---
# <a name="bitsadmin-makecustomheaderswriteonly"></a>bitsadmin makecustomheaderswriteonly

Faça com que os cabeçalhos HTTP personalizados de um trabalho somente sejam gravados.

> [!IMPORTANT]
> Esta ação não pode ser desfeita.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /makecustomheaderswriteonly <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para fazer com que cabeçalhos HTTP personalizados sejam gravados somente para o trabalho chamado *myDownloadJob*:

```
bitsadmin /makecustomheaderswriteonly myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
