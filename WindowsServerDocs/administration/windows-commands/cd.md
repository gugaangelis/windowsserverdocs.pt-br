---
title: cd
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 932d9cc1-3dff-40da-835c-1cb0894874f1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ed0942232eb205a8198d4b3d366ca9482af1f4b3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379710"
---
# <a name="cd"></a>cd



Exibe o nome ou altera o diretório atual. Se usado com apenas uma letra de unidade (por exemplo, `cd C:`), **CD** exibe os nomes do diretório atual na unidade especificada. Se usado sem parâmetros, **CD** exibe a unidade e o diretório atuais.

> [!NOTE]
> Esse comando é o mesmo que o comando **chdir** .

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
cd [/d] [<Drive>:][<Path>]
cd [..]
chdir [/d] [<Drive>:][<Path>]
chdir [..]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/d|Altera a unidade atual, bem como o diretório atual de uma unidade.|
|> da unidade de \<:|Especifica a unidade a ser exibida ou alterada (se for diferente da unidade atual).|
|\<caminho >|Especifica o caminho para o diretório que você deseja exibir ou alterar.|
|[..]|Especifica que você deseja alterar para a pasta pai.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

Se as extensões de comando estiverem habilitadas, as seguintes condições se aplicarão ao comando **CD** :
- A cadeia de caracteres do diretório atual é convertida para usar o mesmo caso que os nomes no disco. Por exemplo, `cd C:\TEMP` definiria o diretório atual como C:\Temp se esse for o caso no disco.
- Os espaços não são tratados como delimitadores, portanto, o *caminho* pode conter espaços sem aspas delimitados. Por exemplo:  
  ```
  cd username\programs\start menu
  ```  
  é o mesmo que:  
  ```
  cd "username\programs\start menu"
  ```  
  As aspas são obrigatórias, no entanto, se as extensões estiverem desabilitadas.

Para desabilitar as extensões de comando, digite:
```
cmd /e:off
```

## <a name="BKMK_examples"></a>Disso

O diretório raiz é a parte superior da hierarquia de diretório de uma unidade. Para retornar ao diretório raiz, digite:
```
cd\
```
Para alterar o diretório padrão em uma unidade diferente daquele em que você está, digite:
```
cd [<Drive>:\[<Directory>]]
```
Para verificar a alteração no diretório, digite:
```
cd [<Drive>:]
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)