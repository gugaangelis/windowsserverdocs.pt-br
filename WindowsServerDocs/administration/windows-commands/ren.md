---
title: ren
description: Saiba como renomear um arquivo ou diretório com o comando ren.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 60398e12-a05d-4524-a73a-0a925943e21d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 235497b09f44f9077b7f622f7f2b68a0bc49af86
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836039"
---
# <a name="ren"></a>ren

Renomeia arquivos ou diretórios. Esse comando é o mesmo que o comando **renomear** .

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
ren [<Drive>:][<Path>]<FileName1> <FileName2>
rename [<Drive>:][<Path>]<FileName1> <FileName2>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[\<drive >:] [\<Path >]\<arquivo1 >|Especifica o local e o nome do arquivo ou conjunto de arquivos que você deseja renomear. *Arquivo1* pode incluir caracteres curinga ( **&#42;** e **?** ).|
|\<arquivo2 >|Especifica o novo nome para o arquivo. Você pode usar caracteres curinga para especificar novos nomes para vários arquivos.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

- Você não pode especificar uma nova unidade ou caminho ao renomear arquivos.
- Você não pode usar o comando **Ren** para renomear arquivos entre unidades ou para mover arquivos para um diretório diferente.
- Você pode usar caracteres curinga ( **&#42;** e **?** ) em qualquer parâmetro *filename* . Os caracteres representados por caracteres curinga em *arquivo2* serão idênticos aos caracteres correspondentes em *arquivo1*.
- *Arquivo2* deve ser um nome de arquivo exclusivo. Se *arquivo2* corresponder a um nome de arquivo existente, **Ren** exibirá a seguinte mensagem:  
  ```
  Duplicate file name or file not found
  ```

## <a name="examples"></a><a name="BKMK_examples"></a>Disso

Para alterar todas as extensões de nome de arquivo. txt no diretório atual para extensões. doc, digite:
```
ren *.txt *.doc 
```
Para alterar o nome de um diretório de Chap10 para Part10, digite:
```
ren chap10 part10 
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)