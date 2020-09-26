---
title: secedit validate
description: Artigo de referência para o comando de validação do secedit, que valida as configurações de segurança armazenadas em um modelo de segurança.
ms.topic: reference
ms.assetid: 9fb06354-f55a-4ca4-9fbc-9a872eb9b9cf
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 057573cf5bc28a47f5140fb58e23ee1ed4bbc4c1
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388094"
---
# <a name="secedit-validate"></a>/Validate secedit

Valida as configurações de segurança armazenadas em um modelo de segurança (arquivo. inf). A validação de modelos de segurança pode ajudá-lo a determinar se um está corrompido ou definido incorretamente. Modelos de segurança corrompidos ou definidos incorretamente não são aplicados.

## <a name="syntax"></a>Sintaxe

```
secedit /validate <configuration file name>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<configuration file name>` | Obrigatórios. Especifica o caminho e o nome do arquivo para o modelo de segurança que será validado. Os arquivos de log não são atualizados por este comando. |

## <a name="examples"></a>Exemplos

Para verificar se o arquivo Rollback. inf, *secRBKcontoso. inf*, ainda é válido após a reversão, digite:

```
secedit /validate secRBKcontoso.inf
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [secedit/Analyze](secedit-analyze.md)

- [secedit/configure](secedit-configure.md)

- [secedit/export](secedit-export.md)

- [/generaterollback secedit](secedit-generaterollback.md)

- [secedit/Import](secedit-import.md)