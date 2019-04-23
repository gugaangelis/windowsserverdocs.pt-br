---
title: subst
description: Saiba como associar um caminho com uma letra de unidade.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3e69234c-2312-4343-868b-afc1017c622a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 858195de89ca8661cf47c25b6cf9b519cc4efbf8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858067"
---
# <a name="subst"></a>subst



Associa um caminho com uma letra de unidade. Se usado sem parâmetros, **subst** exibe os nomes das unidades virtuais em vigor.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
subst [<Drive1>: [<Drive2>:]<Path>] 
subst <Drive1>: /d
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Drive1>:|Especifica a unidade virtual à qual você deseja atribuir um caminho.|
|[\<Drive2>:]\<Path>|Especifica a unidade física e o caminho que você deseja atribuir a uma unidade virtual.|
|/d|Exclui uma unidade já substituída (virtual).|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Os comandos a seguir não funcionam e não deve ser usados em unidades que são especificadas na **subst** comando:

    **chkdsk**

    **diskcomp**

    **diskcopy**

    **format**

    **label**

    **recover**
-   O *unidade1* parâmetro deve estar dentro do intervalo especificado pelo **lastdrive** comando. Caso contrário, **subst** exibe a seguinte mensagem de erro:

    `Invalid parameter - drive1:`

## <a name="BKMK_examples"></a>Exemplos

Para criar uma unidade virtual Z para o caminho B:\User\Betty\Forms, digite:
```
subst z: b:\user\betty\forms 
```
Em vez de digitar o caminho completo, você pode acessar este diretório digitando a letra da unidade virtual seguida por dois-pontos, da seguinte maneira:
```
z: 
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)