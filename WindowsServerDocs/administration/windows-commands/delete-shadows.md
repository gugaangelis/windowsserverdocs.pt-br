---
title: Excluir sombras
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 1a0945477bc4fce907b5ec4a697c7a2ec2f59557
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436112"
---
# <a name="delete-shadows"></a>Excluir sombras



Exclusões de cópias de sombra.

## <a name="syntax"></a>Sintaxe

```
delete shadows [all | volume <Volume> | oldest <Volume> | set <SetID> | id <ShadowID> | exposed {<Drive> | <MountPoint>}]
```

## <a name="parameters"></a>Parâmetros

|     Parâmetro     |                                                                             Descrição                                                                              |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        all        |                                                                      Exclui todas as cópias de sombra.                                                                      |
| volume \<Volume>  |                                                            Exclui todas as cópias de sombra do volume especificado.                                                            |
| mais antigo \<Volume >  |                                                         Exclui a cópia de sombra mais antiga do volume fornecido.                                                          |
|   definir \<SetID >    | Exclui as cópias de sombra no conjunto de cópia de sombra da ID especificada. Você pode especificar um alias usando o **%** se o alias que existe no ambiente atual de símbolo. |
|  ID \<ShadowID >   |              Exclui uma cópia de sombra da ID especificada. Você pode especificar um alias usando o **%** se o alias que existe no ambiente atual de símbolo.               |
| exposto {\<Drive > |                                                                            <MountPoint>}                                                                             |

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)