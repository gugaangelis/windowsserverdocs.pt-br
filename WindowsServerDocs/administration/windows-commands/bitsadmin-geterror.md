---
title: bitsadmin geterror
description: O tópico de comandos do Windows para **GetError Bitsadmin**, que recupera informações de erro detalhadas para o trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cbe5bca1-d2dd-4ce6-903f-f85de4a2ec6a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7c65b072bb190e3e516b917c310942146bb3f3d2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850699"
---
# <a name="bitsadmin-geterror"></a>bitsadmin geterror

Recupera informações detalhadas de erro para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /geterror <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir recupera as informações de erro para o trabalho chamado *myDownloadJob*.

```
C:\>bitsadmin /geterror myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)