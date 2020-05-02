---
title: subst
description: Saiba como associar um caminho a uma letra da unidade.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3e69234c-2312-4343-868b-afc1017c622a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 62ba0de33e69998e7d3e343b1e53c1de7e630e10
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721609"
---
# <a name="subst"></a>subst



Associa um caminho a uma letra da unidade. Se usado sem parâmetros, **subst** exibirá os nomes das unidades virtuais em vigor.



## <a name="syntax"></a>Sintaxe

```
subst [<Drive1>: [<Drive2>:]<Path>] 
subst <Drive1>: /d
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<> unidade1:|Especifica a unidade virtual à qual você deseja atribuir um caminho.|
|[\<Unidade2>:] \<Caminho>|Especifica a unidade física e o caminho que você deseja atribuir a uma unidade virtual.|
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

## <a name="examples"></a><a name="BKMK_examples"></a>Disso

Para criar uma unidade virtual Z para o caminho B:\User\Betty\Forms, digite:
```
subst z: b:\user\betty\forms 
```
Em vez de digitar o caminho completo, você pode acessar esse diretório digitando a letra da unidade virtual seguida por dois-pontos, da seguinte maneira:
```
z: 
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)