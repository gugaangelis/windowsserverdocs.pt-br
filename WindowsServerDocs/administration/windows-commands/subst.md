---
title: subst
description: Saiba como associar um caminho a uma letra da unidade.
ms.topic: reference
ms.assetid: 3e69234c-2312-4343-868b-afc1017c622a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 680a3118d284d18946980ff8dc246ce08b5ea727
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024610"
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
|\<Drive1>:|Especifica a unidade virtual à qual você deseja atribuir um caminho.|
|[\<Drive2>:]\<Path>|Especifica a unidade física e o caminho que você deseja atribuir a uma unidade virtual.|
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

## <a name="examples"></a><a name="BKMK_examples"></a>Exemplos

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