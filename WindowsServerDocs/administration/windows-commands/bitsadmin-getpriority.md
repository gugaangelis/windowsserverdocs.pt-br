---
title: getanteriority Bitsadmin
description: O tópico de comandos do Windows para **getbitsadminion anterior**, que recupera a prioridade do trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: b27829a0fb852abb88c88a65e61e8d7693ca2df2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850539"
---
# <a name="bitsadmin-getpriority"></a>getanteriority Bitsadmin

Recupera a prioridade do trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getpriority <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="remarks"></a>Comentários

A prioridade para esse comando pode ser:

- **FRENTE**

- **ELEVADA**

- **NORMAL**

- **PEQUENA**

- **CONHECIDOS**

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir recupera a prioridade para o trabalho chamado *myDownloadJob*.

```
C:\>bitsadmin /getpriority myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
