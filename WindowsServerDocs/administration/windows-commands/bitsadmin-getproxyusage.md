---
title: bitsadmin getproxyusage
description: Artigo de referência para o comando Bitsadmin getproxyusage, que recupera a configuração de uso de proxy para o trabalho especificado.
ms.topic: reference
ms.assetid: f940a70e-3b02-497e-a47f-b37b905c299e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 423168d72a88ecad90fd8de43eab3cb4486a750b
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024400"
---
# <a name="bitsadmin-getproxyusage"></a>bitsadmin getproxyusage

Recupera a configuração de uso de proxy para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getproxyusage <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

#### <a name="output"></a>Saída

Os valores de uso de proxy retornados podem ser:

- **Preconfig** -use os padrões do Internet Explorer do proprietário.

- **No_Proxy** -não use um servidor proxy.

- **Substituir** – use uma lista de proxy explícita.

- **Detecção automática** – detectar automaticamente as configurações de proxy.

## <a name="examples"></a>Exemplos

Para recuperar o uso do proxy para o trabalho chamado *myDownloadJob*:

```
bitsadmin /getproxyusage myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
