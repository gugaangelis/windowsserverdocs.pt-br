---
title: ren
description: Saiba como renomear um arquivo ou diretório com o comando ren.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 60398e12-a05d-4524-a73a-0a925943e21d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 34c761cb08916d277f8f7f1c58d57a05ed2c8daf
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441813"
---
# <a name="ren"></a>ren

Renomeia a arquivos ou diretórios. Esse comando é igual a **Renomear** comando.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
ren [<Drive>:][<Path>]<FileName1> <FileName2>
rename [<Drive>:][<Path>]<FileName1> <FileName2>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[\<Drive>:][\<Path>]\<FileName1>|Especifica o local e o nome do arquivo ou conjunto de arquivos que você deseja renomear. *Arquivo1* pode incluir caracteres curinga ( **&#42;** e **?** ).|
|\<FileName2>|Especifica o novo nome para o arquivo. Você pode usar caracteres curinga para especificar os novos nomes para vários arquivos.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

- Você não pode especificar uma nova unidade ou caminho ao renomear arquivos.
- Não é possível usar o **ren** comando para renomear arquivos entre unidades ou mover arquivos para um diretório diferente.
- Você pode usar caracteres curinga ( **&#42;** e **?** ) em um *FileName* parâmetro. Caracteres que são representados por caracteres curinga em *FileName2* serão idênticos aos caracteres correspondentes nos *arquivo1*.
- *FileName2* deve ser um nome de arquivo exclusivo. Se *FileName2* corresponde a um nome de arquivo existente **ren** exibe a seguinte mensagem:  
  ```
  Duplicate file name or file not found
  ```

## <a name="BKMK_examples"></a>Exemplos

Para alterar todas as extensões de nome de arquivo. txt no diretório atual para extensões. doc, digite:
```
ren *.txt *.doc 
```
Para alterar o nome de um diretório de Cap10 para Parte10, digite:
```
ren chap10 part10 
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)