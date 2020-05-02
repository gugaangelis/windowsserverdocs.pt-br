---
title: add
description: Tópico de referência para o comando Add, que adiciona volumes ao conjunto de volumes que devem ser copiados por sombra ou adiciona aliases ao ambiente de alias.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 47efce7a-86d2-4872-ae31-baa108757afd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9b621a3061c4e3366085c5cc44f91f26dd33d4e3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719002"
---
# <a name="add"></a>add

Adiciona volumes ao conjunto de volumes que devem ser copiados em sombra ou adiciona aliases ao ambiente de alias. Se usado sem subcomandos, **Adicionar** lista os volumes e aliases atuais.

> [!NOTE]
> Os aliases não são adicionados ao ambiente de alias até que a cópia de sombra seja criada. Os aliases que você precisa imediatamente devem ser adicionados usando **Add alias**.

## <a name="syntax"></a>Sintaxe

```
add
add volume <volume> [provider <providerid>]
add alias <aliasname> <aliasvalue>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| ---------- | ----------- |
| volume | Adiciona um volume ao conjunto de cópias de sombra, que é o conjunto de volumes a serem copiados em sombra. Consulte [Adicionar volume](add-volume.md) para sintaxe e parâmetros. |
| alias | Adiciona o nome e o valor fornecidos ao ambiente do alias. Consulte [Adicionar alias](add-alias.md) para sintaxe e parâmetros. |
| /? | Exibe a ajuda na linha de comando. |

## <a name="examples"></a>Exemplos

Para exibir os volumes adicionados e os aliases que estão atualmente no ambiente, digite:

```
add
```

A saída a seguir mostra que a unidade C foi adicionada ao conjunto de cópias de sombra:

```
Volume c: alias System1    GUID \\?\Volume{XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX}\
1 volume in Shadow Copy Set.
No Diskshadow aliases in the environment.
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)