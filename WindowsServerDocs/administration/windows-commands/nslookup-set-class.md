---
title: nslookup set class
description: Artigo de referência para o comando Set Class do nslookup, que altera a classe de consulta.
ms.topic: reference
ms.assetid: ed826400-40da-42b6-b7f0-95db73790723
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 676c9dad9d01c6889ec472d68df831ba3eec15c7
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038777"
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
