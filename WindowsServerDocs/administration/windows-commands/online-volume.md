---
title: online volume
description: Artigo de referência para o comando de volume online, que coloca o volume offline no estado online.
ms.topic: article
ms.assetid: 5da073fd-578d-4691-ad0f-605ba66e0c7e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b09730d3cc0cfe758c90c3ca57fd039282ba3fce
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87885220"
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
