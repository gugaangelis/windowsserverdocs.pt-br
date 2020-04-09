---
title: bitsadmin getdescription
description: O tópico de comandos do Windows para **GetDescription Bitsadmin**, que recupera a descrição do trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f3974603-ebbe-4d31-8217-040fe2d90c85
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ff1638cf634d76001042691fd890dfe41f9ae0b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850719"
---
# <a name="bitsadmin-getdescription"></a>bitsadmin getdescription

Recupera a descrição do trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getdescription <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir recupera a descrição para o trabalho chamado *myDownloadJob*.

```
C:\>bitsadmin /getdescription myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)