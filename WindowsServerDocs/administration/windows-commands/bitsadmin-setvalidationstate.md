---
title: Bitsadmin setvalidable
description: O tópico de comandos do Windows para Bitsadmin setvalidable, que define o estado de validação de conteúdo do arquivo fornecido dentro do trabalho.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e8fc8e8c-171c-4681-8057-6986b018e576
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: de6480596b55b3a483076297f32ce52a975915db
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849109"
---
# <a name="bitsadmin-setvalidationstate"></a>Bitsadmin setvalidable

Define o estado de validação de conteúdo do arquivo fornecido dentro do trabalho.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetValidationState <Job> <file index> <true|false> 
```

### <a name="parameters"></a>Parâmetros

| Parâmetro  |          Descrição           |
|------------|--------------------------------|
|    Trabalho     | O nome de exibição ou o GUID do trabalho |
| Índice de arquivo |         Começa com 0          |
|    True    |             False              |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir define o estado de validação de conteúdo do arquivo 2 como TRUE para o trabalho chamado *myJob*.
```
C:\>bitsadmin /SetValidationState myJob 2 TRUE 
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)