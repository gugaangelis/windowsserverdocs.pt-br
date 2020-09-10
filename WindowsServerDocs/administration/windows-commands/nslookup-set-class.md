---
title: nslookup set class
description: Artigo de referência para o comando Set Class do nslookup, que altera a classe de consulta.
ms.topic: reference
ms.assetid: ed826400-40da-42b6-b7f0-95db73790723
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 5f4eb309c3972230247bf231b0e731e146d8159a
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89634035"
---
# <a name="nslookup-set-class"></a>nslookup set class

Altera a classe de consulta. A classe especifica o grupo de protocolo das informações.

## <a name="syntax"></a>Sintaxe

```
set class=<class>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<class>` | Os valores válidos incluem:<ul><li>**Em:** Especifica a classe da Internet. Esse é o valor padrão.</li><li>**Caos:** Especifica a classe de caos.</li><li>**HESIOD:** Especifica a classe Hesiod do MIT Athena.</li><li>**Qualquer:** Especifica o uso de qualquer um dos valores listados anteriormente.</li></ul> |
| /? | Exibe a ajuda no prompt de comando. |
| /help | Exibe a ajuda no prompt de comando. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
