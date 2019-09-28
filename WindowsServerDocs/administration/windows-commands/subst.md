---
title: subst
description: Saiba como associar um caminho a uma letra da unidade.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: f3010d1e58fbd360b8311512e6664873b020c12b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383746"
---
# <a name="subst"></a>subst



Associa um caminho a uma letra da unidade. Se usado sem parâmetros, **subst** exibirá os nomes das unidades virtuais em vigor.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
subst [<Drive1>: [<Drive2>:]<Path>] 
subst <Drive1>: /d
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|> de @no__t 0Drive1:|Especifica a unidade virtual à qual você deseja atribuir um caminho.|
|[\<Drive2 >:] \<Path >|Especifica a unidade física e o caminho que você deseja atribuir a uma unidade virtual.|
|/d|Exclui uma unidade substituída (virtual).|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Os comandos a seguir não funcionam e não devem ser usados em unidades especificadas no comando **subst** :

    **chkdsk**

    **diskcomp**

    **diskcopy**

    **format**

    **label**

    **recover**
-   O parâmetro *unidade1* deve estar dentro do intervalo especificado pelo comando **LastDrive** . Caso contrário, **subst** exibirá a seguinte mensagem de erro:

    `Invalid parameter - drive1:`

## <a name="BKMK_examples"></a>Disso

Para criar uma unidade virtual Z para o caminho B:\User\Betty\Forms, digite:
```
subst z: b:\user\betty\forms 
```
Em vez de digitar o caminho completo, você pode acessar esse diretório digitando a letra da unidade virtual seguida por dois-pontos, da seguinte maneira:
```
z: 
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)