---
title: expose
description: Artigo de referência para o comando de exposição, que expõe uma cópia de sombra persistente como uma letra de unidade, um compartilhamento ou um ponto de montagem.
ms.topic: reference
ms.assetid: 9b0a21cf-3bef-4ade-b8f1-ac42f9203947
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 3d36ec0a1c4f85282c1949700dad1f4568356748
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635902"
---
# <a name="expose"></a>expose

Expõe uma cópia de sombra persistente como uma letra de unidade, um compartilhamento ou um ponto de montagem.

## <a name="syntax"></a>Sintaxe

```
expose <shadowID> {<drive:> | <share> | <mountpoint>}
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| shadowid | Especifica a ID de sombra da cópia de sombra que você deseja expor. Você também pode usar um alias existente ou uma variável de ambiente no lugar de *shadowid*. Use **Adicionar** sem parâmetros para ver os aliases existentes. |
| `<drive:>` | Expõe a cópia de sombra especificada como uma letra da unidade (por exemplo, `p:` ). |
| `<share>` | Expõe a cópia de sombra especificada em um compartilhamento (por exemplo, `\\machinename` ).   |
| `<mountpoint>` | Expõe a cópia de sombra especificada para um ponto de montagem (por exemplo, `C:\shadowcopy` ). |

### <a name="examples"></a>Exemplos

Para expor a cópia de sombra persistente associada à variável de ambiente VSS_SHADOW_1 como a unidade X, digite:

```
expose %vss_shadow_1% x:
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando DiskShadow](diskshadow.md)
