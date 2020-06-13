---
title: nslookup set class
description: Tópico de referência para o comando Set Class do nslookup, que altera a classe de consulta.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ed826400-40da-42b6-b7f0-95db73790723
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e939be13eedcab557dc6dcbe16f2e83f810c20d5
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721559"
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
