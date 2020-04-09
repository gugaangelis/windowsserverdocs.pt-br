---
title: Bitsadmin getproxylist – recupera a lista de proxy para o trabalho especificado.
description: Tópico de comandos do Windows para **Bitsadmin getproxylist**, que recupera a lista de proxy para o trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: eebfa727-d8f1-4ae3-9382-6d8ffe8c3df3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0c0d26fb074bd1b792caa7fe2ce8fd31b64365e2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850519"
---
# <a name="bitsadmin-getproxylist"></a>bitsadmin getproxylist

Recupera a lista delimitada por vírgulas de servidores proxy a serem usados para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getproxylist <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir recupera a lista de proxy para o trabalho chamado *myDownloadJob*.

```
C:\>bitsadmin /getproxylist myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)