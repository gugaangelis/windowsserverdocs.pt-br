---
title: ftp literal
description: Artigo de referência para o comando FTP literal, que envia argumentos textuais para o servidor FTP remoto.
ms.topic: article
ms.assetid: fb81aa2d-07fa-4e79-bf44-1fb5526fdf14
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bafa2626481941b91d501e4fd6df52aa1f8f05d1
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87889353"
---
# <a name="ftp-literal"></a>ftp literal

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envia argumentos textuais para o servidor FTP remoto. Um único código de resposta FTP é retornado.

> [!NOTE]
> Esse comando é o mesmo que o [comando de aspas de FTP](ftp-quote.md).

## <a name="syntax"></a>Sintaxe

```
literal <argument> [ ]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<argument>` | Especifica o argumento a ser enviado ao servidor FTP. |

### <a name="examples"></a>Exemplos

Para enviar um comando **Quit** para o servidor FTP remoto, digite:

```
literal quit
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando de aspas de FTP](ftp-quote.md)

- [Diretrizes adicionais de FTP](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
