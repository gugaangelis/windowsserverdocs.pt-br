---
title: cache Bitsadmin e DeleteUrl
description: Tópico de comandos do Windows para **cache Bitsadmin e DeleteUrl**, que exclui todas as entradas de cache para a URL fornecida.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e108b76b-fae9-4c16-bf4c-d74c9f025953
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 70099e795d0f05d0fcf75fbf6b82f5466d1c0c55
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850929"
---
# <a name="bitsadmin-cache-and-deleteurl"></a>cache Bitsadmin e DeleteUrl

Exclui todas as entradas de cache para a URL fornecida.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /deleteURL url
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| url | O localizador de recursos uniforme que identifica um arquivo remoto. |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir exclui todas as entradas de cache para `https://www.contoso.com/en/us/default.aspx`

```
C:\>bitsadmin /deleteURL https://www.contoso.com/en/us/default.aspx 
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)