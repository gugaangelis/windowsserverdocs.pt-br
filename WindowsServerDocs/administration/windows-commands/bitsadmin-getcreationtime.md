---
title: bitsadmin getcreationtime
description: O tópico de comandos do Windows para **Bitsadmin GetCreationTime**, que recupera a hora de criação para o trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: be409cb5-ce72-41d9-aafa-edd4e230fd14
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cd8f718e53cc44dc5f4c6f5ff09c9a5c201e0564
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850739"
---
# <a name="bitsadmin-getcreationtime"></a>bitsadmin getcreationtime

Recupera a hora de criação para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getcreationtime <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir recupera a hora de criação para o trabalho chamado *myDownloadJob*.

```
C:\>bitsadmin /getcreationtime myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)