---
title: excluir sombras
description: Tópico de referência para excluir sombras, que exclui cópias de sombra.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e29a84d2-04d1-4eb1-910a-5a47bddbc24d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1dd367d76ad1699321af9caf47a0ddc351088a05
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720804"
---
# <a name="delete-shadows"></a>excluir sombras

Exclui cópias de sombra.

## <a name="syntax"></a>Sintaxe

```
delete shadows [all | volume <Volume> | oldest <Volume> | set <SetID> | id <ShadowID> | exposed {<Drive> | <MountPoint>}]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| ---- | ---- |
| all | Exclui todas as cópias de sombra. |
| \<volume do volume> | Exclui todas as cópias de sombra do volume especificado. |
| > \<de volume mais antigo | Exclui a cópia de sombra mais antiga do volume especificado. |
| definir \<SetID> | Exclui as cópias de sombra no conjunto de cópias de sombra da ID fornecida. Você pode especificar um alias usando o **%** símbolo se o alias existir no ambiente atual. |
| ID \<de shadowid> | Exclui uma cópia de sombra da ID fornecida. Você pode especificar um alias usando o **%** símbolo se o alias existir no ambiente atual. |
| exposto {\<Drive> | <MountPoint>} |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)