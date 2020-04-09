---
title: bitsadmin gethelpertokensid
description: O tópico de comandos do Windows para **Bitsadmin gethelpertokensid**, que retorna o Sid de um token auxiliar do trabalho de transferência de bits, se um estiver definido.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: a2e26ff459b068595529fbd24e6165c130660570
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850639"
---
# <a name="bitsadmin-gethelpertokensid"></a>bitsadmin gethelpertokensid

Retorna o SID de um [token auxiliar](https://docs.microsoft.com/windows/win32/bits/helper-tokens-for-bits-transfer-jobs)do trabalho de transferência de bits, se um estiver definido.

> [!NOTE]
> Esse comando não tem suporte no BITS 3,0 e versões anteriores.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /gethelpertokensid <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
