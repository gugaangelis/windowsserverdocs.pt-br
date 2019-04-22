---
title: Adicionar volume
description: Tópico de comandos do Windows para **Adicionar volume** -adiciona volumes para a cópia de sombra copiado do conjunto, que é o conjunto de volumes de backup de sombra.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b7d4d35d-8bda-46d2-8df5-eb598cecaaba
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8960ffafdf49d4512e1df2dfcc046bdfbe56e224
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819467"
---
# <a name="add-volume"></a>Adicionar volume



Adiciona os volumes para o conjunto de cópias de sombra, que é o conjunto de volumes a serem copiados em sombra. Este comando é necessário para criar cópias de sombra. Se usado sem parâmetros, **Adicionar volume** exibe a Ajuda no prompt de comando.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
add volume <Volume> [provider <ProviderID>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Volume>|Especifica um volume para adicionar ao conjunto de cópia de sombra. Pelo menos um volume é necessário para a criação de cópias de sombra.|
|[provedor \<ProviderID >]|Especifica a ID do provedor de um provedor registrado para usar para criar a cópia de sombra. Se **provedor** não for especificado, o provedor padrão é usado.|

## <a name="remarks"></a>Comentários

-   Os volumes são adicionados um por vez.
-   Cada vez que um volume é adicionado, ele será verificado para garantir que o VSS oferece suporte à criação de cópias de sombra do volume. Essa verificação primária pode ser invalidada, no entanto, por uso posterior da **definir o contexto de** comando.
-   Quando uma cópia de sombra é criada, uma variável de ambiente vincula o alias para a ID de sombra, portanto, o alias pode ser usado para execução de scripts.

## <a name="BKMK_examples"></a>Exemplos

Para exibir a lista atual de provedores registrados, no `DISKSHADOW>` prompt, digite:
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
Para adicionar a unidade C para o conjunto de cópias de sombra e atribuir um alias chamado System1, digite:
```
add volume c: alias System1
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)