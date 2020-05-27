---
title: add alias
description: Tópico de referência para o comando Add alias, que adiciona aliases ao ambiente de alias.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5fe12f5d-11e9-4f3d-b7f9-40b26c8685e5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 66301a39a1e969e270b42b5ce92a73392a134357
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2020
ms.locfileid: "83819656"
---
# <a name="add-alias"></a>add alias

Adiciona aliases ao ambiente de alias. Se usado sem parâmetros, **Add alias** exibe a ajuda no prompt de comando. Os aliases são salvos no arquivo de metadados e serão carregados com o comando **carregar metadados** .

## <a name="syntax"></a>Sintaxe

```
add alias <aliasname> <aliasvalue>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<aliasname>` | Especifica o nome do alias. |
| `<aliasvalue>` | Especifica o valor do alias. |
| `? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para listar todas as sombras, incluindo seus aliases, digite:

```
list shadows all
```

O trecho a seguir mostra uma cópia de sombra para a qual o alias padrão, *VSS_SHADOW_x*, foi atribuído:

```
* Shadow Copy ID = {ff47165a-1946-4a0c-b7f4-80f46a309278}
%VSS_SHADOW_1%
```

Para atribuir um novo alias com o nome *System1* a essa cópia de sombra, digite:

```
add alias System1 %VSS_SHADOW_1%
```

Como alternativa, você pode atribuir o alias usando a ID da cópia de sombra:

```
add alias System1 {ff47165a-1946-4a0c-b7f4-80f46a309278}
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando carregar metadados](load-metadata.md)
