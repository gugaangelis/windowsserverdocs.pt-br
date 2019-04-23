---
title: cd
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 5f907b6162e6767820e23222e287b933397397d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861097"
---
# <a name="cd"></a>cd



Exibe o nome da ou altera o diretório atual. Se usado com apenas uma letra de unidade (por exemplo, `cd C:`), **cd** exibe os nomes do diretório atual na unidade especificada. Se usado sem parâmetros, **cd** exibe a unidade atual e o diretório.

> [!NOTE]
> Esse comando é igual a **chdir** comando.

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
|/d|Altera a unidade atual, bem como o diretório atual para uma unidade.|
|\<Drive>:|Especifica a unidade para exibir ou alterar (se for diferente da unidade atual).|
|\<Caminho >|Especifica o caminho para o diretório que você deseja exibir ou alterar.|
|[..]|Especifica que você deseja alterar para a pasta pai.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

Se as extensões de comando estiverem habilitadas, as seguintes condições se aplicam para o **cd** comando:
-   A cadeia de caracteres de diretório atual é convertida para usar da mesma forma como os nomes no disco. Por exemplo, `cd C:\TEMP` definiria o diretório atual como C:\Temp se esse for o caso no disco.
-   Os espaços não são tratados como delimitadores, portanto *caminho* podem conter espaços sem aspas de circunscrição. Por exemplo:   
    ```
    cd username\programs\start menu
    ```  
    é o mesmo que:  
    ```
    cd "username\programs\start menu"
    ```  
    As aspas são necessárias, no entanto, se as extensões estão desabilitadas.

Para desabilitar as extensões de comando, digite:
```
cmd /e:off
```

## <a name="BKMK_examples"></a>Exemplos

O diretório raiz é a parte superior da hierarquia de pastas de uma unidade. Para retornar para o diretório raiz, digite:
```
cd\
```
Para alterar o diretório padrão em uma unidade diferente daquele que você estiver usando, digite:
```
cd [<Drive>:\[<Directory>]]
```
Para verificar a alteração para o diretório, digite:
```
cd [<Drive>:]
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)