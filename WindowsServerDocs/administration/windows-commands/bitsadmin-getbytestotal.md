---
title: bitsadmin getbytestotal
description: Tópico de comandos do Windows para **Bitsadmin getbytestotal**, que recupera o tamanho do trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 784e0bfa-7b09-4262-9104-adbc9beb479b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 84e3e0311ead0eb79f9247d4f06844ece5f20fa2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850779"
---
# <a name="bitsadmin-getbytestotal"></a>bitsadmin getbytestotal

Recupera o tamanho do trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getbytestotal <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir recupera o tamanho do trabalho chamado *myDownloadJob*.

```
C:\>bitsadmin /getbytestotal myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)