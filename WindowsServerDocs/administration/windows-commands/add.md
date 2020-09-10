---
title: add
description: Artigo de referência para o comando Add, que adiciona volumes ao conjunto de volumes que devem ser copiados por sombra ou adiciona aliases ao ambiente de alias.
ms.topic: reference
ms.assetid: 47efce7a-86d2-4872-ae31-baa108757afd
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ddd090323b45caa0cc7afa4c07c568793f2e268d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633498"
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