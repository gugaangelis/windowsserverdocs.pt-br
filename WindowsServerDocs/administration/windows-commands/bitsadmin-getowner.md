---
title: bitsadmin getowner
description: Tópico de comandos do Windows para Bitsadmin **GetOwner**, que recupera o proprietário do trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5203f84c-a879-4f31-ae3e-7ea74bd63ca5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3e622c3759c9ec20867c693539c4481c70aa4f26
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850559"
---
# <a name="bitsadmin-getowner"></a>bitsadmin getowner

Exibe o nome de exibição ou o GUID do proprietário do trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getowner <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir exibe o proprietário do trabalho chamado *myDownloadJob*.

```
C:\>bitsadmin /getowner myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)