---
title: Expo
description: Tópico de referência para o comando de exposição, que expõe uma cópia de sombra persistente como uma letra de unidade, um compartilhamento ou um ponto de montagem.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9b0a21cf-3bef-4ade-b8f1-ac42f9203947
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d4e8ebf71f6ddcb457460f8174793586e81c73a6
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2020
ms.locfileid: "83437171"
---
# <a name="expose"></a>Expo

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
