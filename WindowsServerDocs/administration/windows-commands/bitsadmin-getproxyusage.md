---
title: bitsadmin getproxyusage
description: Tópico de comandos do Windows para **Bitsadmin getproxyusage**, que recupera a configuração de uso de proxy para o trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f940a70e-3b02-497e-a47f-b37b905c299e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 01c9bb9a1d413fa847482f652e18eed30ad76109
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850509"
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

## <a name="remarks"></a>Comentários

Os valores de uso de proxy incluem:

- **Preconfig** -use os padrões do Internet Explorer do proprietário.

- **No_Proxy** -não use um servidor proxy.

- **Substituir** – use uma lista de proxy explícita.

- **Detecção automática** – detectar automaticamente as configurações de proxy.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir recupera o uso de proxy para o trabalho chamado *myDownloadJob*.

```
C:\>bitsadmin /getproxyusage myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)