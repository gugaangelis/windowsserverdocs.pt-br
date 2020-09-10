---
title: ftp quote
description: Artigo de referência para o comando de aspas de FTP, que envia argumentos textuais para o servidor FTP remoto.
ms.topic: reference
ms.assetid: 4500a1d3-c091-42c7-a909-f61df7f2e993
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 985e32342aea9cee7a658709fb83c7efac281308
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89624639"
---
# <a name="ftp-quote"></a>ftp quote

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envia argumentos textuais para o servidor FTP remoto. Um único código de resposta FTP é retornado.

> [!NOTE]
> Esse comando é o mesmo que o [comando FTP literal](ftp-literal_1.md).

## <a name="syntax"></a>Sintaxe

```
quote <argument>[ ]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<argument>` | Especifica o argumento a ser enviado ao servidor FTP. |

### <a name="examples"></a>Exemplos

Para enviar um comando **Quit** para o servidor FTP remoto, digite:

```
quote quit
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando FTP literal](ftp-literal_1.md)

- [Diretrizes adicionais de FTP](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
