---
title: telnet send
description: Artigo de referência para o comando telnet Send, que envia comandos Telnet para o servidor Telnet.
ms.topic: reference
ms.assetid: 7c217abc-1182-466e-914c-1ff16755021b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ab6c94f619ab28be7b850479a7e3d476e1394e09
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91717983"
---
# <a name="telnet-send"></a>Telnet: enviar

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envia comandos Telnet para o servidor Telnet.

## <a name="syntax"></a>Sintaxe

```
sen {ao | ayt | brk | esc | ip | synch | <string>} [?]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| ao | Envia a saída de **anulação**do comando telnet. |
| ayt | Envia o comando telnet **?** |
| brk | Envia o comando telnet **BRK**. |
| esc | Envia o caractere de escape Telnet atual. |
| IP | Envia o processo de **interrupção**do comando telnet. |
| sincronização | Envia a sincronização do comando telnet. |
| `<string>` | Envia qualquer cadeia de caracteres que você digitar para o servidor Telnet. |
| ? | Exibe a ajuda associada a este comando. |

## <a name="example"></a>Exemplo

Para enviar o comando **você está lá?** para o servidor Telnet, digite:

```
sen ayt
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
