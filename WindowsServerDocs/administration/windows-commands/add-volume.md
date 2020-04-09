---
title: Adicionar volume
description: O tópico de comandos do Windows para **Adicionar volume**, que adiciona volumes ao conjunto de cópias de sombra, que é o conjunto de volumes a serem copiados em sombra.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b7d4d35d-8bda-46d2-8df5-eb598cecaaba
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 806ab273dbb63eb7341520f56a07691fe3fac214
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851349"
---
# <a name="add-volume"></a>Adicionar volume

Adiciona volumes ao conjunto de cópias de sombra, que é o conjunto de volumes a serem copiados em sombra. Esse comando é necessário para criar cópias de sombra. Se usado sem parâmetros, **Adicionar volume** exibirá a ajuda no prompt de comando.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
add volume <Volume> [provider <ProviderID>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
| `<Volume>` | Especifica um volume a ser adicionado ao conjunto de cópias de sombra. Pelo menos um volume é necessário para a criação da cópia de sombra.|
| `[provider \<ProviderID>]` | Especifica a ID do provedor de um provedor registrado a ser usado para criar a cópia de sombra. Se o **provedor** não for especificado, o provedor padrão será usado.|

## <a name="remarks"></a>Comentários

-   Os volumes são adicionados um de cada vez.

-   Cada vez que um volume é adicionado, ele é verificado para garantir que o VSS dê suporte à criação de cópia de sombra desse volume. No entanto, essa verificação primária pode ser invalidada pelo uso posterior do comando **set Context** .

-   Quando uma cópia de sombra é criada, uma variável de ambiente vincula o alias à ID de sombra, portanto, o alias pode ser usado para scripts.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para exibir a lista atual de provedores registrados, no prompt de `DISKSHADOW>`, digite:

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

Para adicionar a unidade C ao conjunto de cópias de sombra e atribuir um alias chamado System1, digite:

```
add volume c: alias System1
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)