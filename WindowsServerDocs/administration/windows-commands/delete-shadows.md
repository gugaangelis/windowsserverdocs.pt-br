---
title: excluir sombras
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e29a84d2-04d1-4eb1-910a-5a47bddbc24d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c965af8b045c5ab3a110542d148b255f382a95c3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378631"
---
# <a name="delete-shadows"></a>excluir sombras



exclui cópias de sombra.

## <a name="syntax"></a>Sintaxe

```
delete shadows [all | volume <Volume> | oldest <Volume> | set <SetID> | id <ShadowID> | exposed {<Drive> | <MountPoint>}]
```

## <a name="parameters"></a>Parâmetros

|     Parâmetro     |                                                                             Descrição                                                                              |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        tudo        |                                                                      Exclui todas as cópias de sombra.                                                                      |
| volume \<volume >  |                                                            Exclui todas as cópias de sombra do volume especificado.                                                            |
| Volume de \<mais antigo >  |                                                         Exclui a cópia de sombra mais antiga do volume especificado.                                                          |
|   definir \<SetID >    | Exclui as cópias de sombra no conjunto de cópias de sombra da ID fornecida. Você pode especificar um alias usando o símbolo de **%** se o alias existir no ambiente atual. |
|  ID \<Shadowid >   |              Exclui uma cópia de sombra da ID fornecida. Você pode especificar um alias usando o símbolo de **%** se o alias existir no ambiente atual.               |
| exposição de {\<drive > |                                                                            <MountPoint>}                                                                             |

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)