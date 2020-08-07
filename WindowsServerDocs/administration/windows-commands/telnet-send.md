---
title: telnet send
description: Artigo de referência do telnet Send, que envia comandos Telnet para o servidor Telnet.
ms.topic: article
ms.assetid: 7c217abc-1182-466e-914c-1ff16755021b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9045707dc156d3169bacfe55e9094c200d6f5166
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87881691"
---
# <a name="telnet-send"></a>Telnet: enviar

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envia comandos Telnet para o servidor Telnet.

## <a name="syntax"></a>Sintaxe
```
sen[d] {ao | ayt | brk | esc | ip | synch | <string>} [?]
```
#### <a name="parameters"></a>Parâmetros

| Parâmetro |                     Descrição                      |
|-----------|------------------------------------------------------|
|    ao     |       Envia a saída de anulação do comando telnet.        |
|    ayt    |       Envia o comando telnet.       |
|    brk    |            Envia o comando telnet BRK.            |
|    esc    |      Envia o caractere de escape Telnet atual.      |
|    IP     |     Envia o processo de interrupção do comando telnet.     |
|   sincronização   |           Envia a sincronização do comando telnet.           |
| <string>  | Envia qualquer cadeia de caracteres que você digitar para o servidor Telnet. |
|     ?     |     Exibe a ajuda associada a este comando.      |

## <a name="examples"></a>Exemplos
Envie-o para o servidor Telnet.
```
sen ayt
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
