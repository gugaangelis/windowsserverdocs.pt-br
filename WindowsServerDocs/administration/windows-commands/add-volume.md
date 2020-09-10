---
title: add volume
description: Artigo de referência para o comando Add volume, que adiciona volumes ao conjunto de cópias de sombra, que é o conjunto de volumes a serem copiados por sombra.
ms.topic: reference
ms.assetid: b7d4d35d-8bda-46d2-8df5-eb598cecaaba
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2a1dc6f00ebc497e9276d1f3b5a74a1f1a10dbfc
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633546"
---
# <a name="add-volume"></a>add volume

Adiciona volumes ao conjunto de cópias de sombra, que é o conjunto de volumes a serem copiados em sombra. Quando uma cópia de sombra é criada, uma variável de ambiente vincula o alias à ID de sombra, portanto, o alias pode ser usado para scripts.

Os volumes são adicionados um de cada vez. Sempre que um volume é adicionado, ele é verificado para garantir que o VSS dê suporte à criação de cópia de sombra para esse volume. Essa verificação pode ser invalidada pelo uso posterior do comando **set Context** .

Esse comando é necessário para criar cópias de sombra. Se usado sem parâmetros, **Adicionar volume** exibirá a ajuda no prompt de comando.

## <a name="syntax"></a>Sintaxe

```
add volume <volume> [provider <providerid>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<volume>` | Especifica um volume a ser adicionado ao conjunto de cópias de sombra. Pelo menos um volume é necessário para a criação da cópia de sombra. |
| `[provider \<providerid>]` | Especifica a ID do provedor de um provedor registrado a ser usado para criar a cópia de sombra. Se o **provedor** não for especificado, o provedor padrão será usado. |

## <a name="examples"></a>Exemplos

Para exibir a lista atual de provedores registrados, no `diskshadow>` prompt, digite:

```
list providers
```

A saída a seguir exibe um único provedor, que será usado por padrão:

```
* ProviderID: {b5946137-7b9f-4925-af80-51abd60b20d5}
        Type: [1] VSS_PROV_SYSTEM
        Name: Microsoft Software Shadow Copy provider 1.0
        Version: 1.0.0.7
        CLSID: {65ee1dba-8ff4-4a58-ac1c-3470ee2f376a}
1 provider registered.
```

Para adicionar a unidade C: ao conjunto de cópias de sombra e atribuir um alias chamado *System1*, digite:

```
add volume c: alias System1
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Definir comando de contexto](set-context.md)
