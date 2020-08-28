---
title: online volume
description: Artigo de referência para o comando de volume online, que coloca o volume offline no estado online.
ms.topic: reference
ms.assetid: 5da073fd-578d-4691-ad0f-605ba66e0c7e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8847f340ebe3e295946550c46fd86bd59de300af
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89032684"
---
# <a name="online-volume"></a>online volume

Coloca o volume offline no estado online. Este comando funciona em volumes que falharam, estão falhando ou estão em estado de redundância com falha.

> [!NOTE]
> Um volume deve ser selecionado para que o comando de **volume online** tenha sucesso. Use o comando [selecionar volume](select-volume.md) para selecionar um volume e deslocar o foco para ele.

> [!IMPORTANT]
> Esse comando falhará se for usado em um disco somente leitura.

## <a name="syntax"></a>Sintaxe

```
online volume [noerr]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| NOERR | Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |

### <a name="examples"></a>Exemplos

Para colocar o volume com foco online, digite:

```
online volume
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
