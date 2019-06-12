---
title: label
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bbae8bdd-97d4-4566-9118-7c95aa07645f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d0c68fbbf3ea776bbf6cd49fc4fa446d5dd46542
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437915"
---
# <a name="label"></a>label



Cria, altera ou exclui o rótulo do volume (ou seja, o nome) de um disco. Se usado sem parâmetros, o **rótulo** comando altera o rótulo do volume atual ou exclui o rótulo existente.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
label [/mp] [<Volume>] [<Label>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/mp|Especifica que o volume deve ser tratado como um nome de volume ou ponto de montagem.|
|\<Volume>|Especifica uma letra de unidade (seguida por dois-pontos), ponto de montagem ou nome do volume. Se for especificado um nome de volume, o **/mp** parâmetro é desnecessário.|
|\<Label>|Especifica o rótulo do volume.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

- Windows exibe o número de série e o rótulo do volume (se houver) como parte da listagem de diretório.
- Um rótulo de volume NTFS pode ser até 32 caracteres, incluindo espaços. Rótulos de volumes NTFS mantêm e exibem e o caso em que foi usado quando o rótulo foi criado.
- Se você não especificar um valor para o **etiqueta** parâmetro, o **rótulo** comando exibe a saída no formato a seguir:  
  ```
  Volume in drive C: xxxxxxxxxxx 
  Volume Serial Number is xxxx-xxxx 
  Volume label (32 characters, ENTER for none)?
  ```  
  Você pode digitar um novo rótulo de volume ou pressione ENTER para manter o rótulo atual. Se você pressionar ENTER e o volume atualmente tiver um rótulo, o **rótulo** comando solicita a você com a seguinte mensagem:  
  ```
  Delete current volume label (Y/N)?
  ```  
  Pressione Y para excluir o rótulo, ou pressione N para manter o rótulo.

## <a name="BKMK_examples"></a>Exemplos

Para rotular um disco na unidade A que contém informações de vendas de julho, digite:
```
label a:sales-july
```
Para excluir o rótulo atual para a unidade C, siga estas etapas:
1. Em um prompt de comando, digite:  
   ```
   Label
   ```  
   Deve ser exibida a saída semelhante à seguinte:  
   ```
   Volume in drive C: is Main Disk
   Volume Serial Number is 6789-ABCD
   Volume label (32 characters, ENTER for none)?
   ```  
2. Pressione ENTER. O seguinte prompt deve ser exibido:  
   ```
   Delete current volume label (Y/N)?
   ```  
3. Pressione Y para excluir o rótulo atual.

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)