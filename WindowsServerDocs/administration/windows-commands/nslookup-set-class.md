---
title: nslookup set class
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ed826400-40da-42b6-b7f0-95db73790723
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b1ae3a5336815a5273aafa976b1dcad8b60fac9b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723651"
---
# <a name="nslookup-set-class"></a>nslookup set class



Altera a classe de consulta. A classe especifica o grupo de protocolo das informações.

## <a name="syntax"></a>Sintaxe

```
set class=<Class>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro |                                                                                                                                    Descrição                                                                                                                                    |
|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| \<Class>  | A classe padrão está em. O a seguir lista os valores válidos para esse comando.</br>-IN: especifica a classe da Internet.</br>-CAOS: especifica a classe de caos.</br>-HESIOD: especifica a classe Hesiod do MIT Athena.</br>-ANY: Especifica qualquer um dos curingas listados anteriormente. |
|   {ajuda   |                                                                                                                                        ?}                                                                                                                                         |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)