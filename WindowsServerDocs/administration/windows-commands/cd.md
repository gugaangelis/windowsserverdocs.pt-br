---
title: cd
description: O tópico comandos do Windows para CD, que exibe o nome ou altera o diretório atual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 932d9cc1-3dff-40da-835c-1cb0894874f1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2d62f529ab6c45957f0fdea24358a2f13151adb6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848219"
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

### <a name="parameters"></a>Parâmetros

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
  cd username\programs\start menu
  ```  
  As aspas são obrigatórias, no entanto, se as extensões estiverem desabilitadas.

Para desabilitar as extensões de comando, digite:
```
cmd /e:off
```

## <a name="examples"></a><a name=BKMK_examples></a>Disso

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

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)