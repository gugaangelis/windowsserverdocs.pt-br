---
title: excluir sombras
description: Tópico de comandos do Windows para excluir sombras, que exclui cópias de sombra.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e29a84d2-04d1-4eb1-910a-5a47bddbc24d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cd109f7ddc0365d03737eddba31a1a4b7f34915b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846559"
---
# <a name="delete-shadows"></a>excluir sombras

exclui cópias de sombra.

## <a name="syntax"></a>Sintaxe

```
delete shadows [all | volume <Volume> | oldest <Volume> | set <SetID> | id <ShadowID> | exposed {<Drive> | <MountPoint>}]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| ---- | ---- |
| all | Exclui todas as cópias de sombra. |
| volume \<volume > | Exclui todas as cópias de sombra do volume especificado. |
| Volume de \<mais antigo > | Exclui a cópia de sombra mais antiga do volume especificado. |
| definir \<SetID > | Exclui as cópias de sombra no conjunto de cópias de sombra da ID fornecida. Você pode especificar um alias usando o símbolo de **%** se o alias existir no ambiente atual. |
| ID \<Shadowid > | Exclui uma cópia de sombra da ID fornecida. Você pode especificar um alias usando o símbolo de **%** se o alias existir no ambiente atual. |
| exposição de {\<drive > | <MountPoint>} |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)