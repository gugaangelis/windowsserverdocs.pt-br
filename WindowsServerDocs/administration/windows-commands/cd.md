---
title: cd
description: Tópico de referência para o comando CD, que exibe o nome ou altera o diretório atual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 932d9cc1-3dff-40da-835c-1cb0894874f1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7c9ee57590cf165ba46f394cab06817c7c13f0a9
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719653"
---
# <a name="cd"></a>cd

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe o nome do diretório atual ou altera o diretório atual. Se usado com apenas uma letra de unidade (por exemplo `cd C:`,), **CD** exibe os nomes do diretório atual na unidade especificada. Se usado sem parâmetros, **CD** exibe a unidade e o diretório atuais.

> [!NOTE]
> Esse comando é o mesmo que o [comando chdir](chdir.md).

## <a name="syntax"></a>Sintaxe

```
cd [/d] [<drive>:][<path>]
cd [..]
chdir [/d] [<drive>:][<path>]
chdir [..]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| /d | Altera a unidade atual, bem como o diretório atual de uma unidade. |
| `<drive>:` | Especifica a unidade a ser exibida ou alterada (se for diferente da unidade atual). |
| `<path>` | Especifica o caminho para o diretório que você deseja exibir ou alterar. |
| [..] | Especifica que você deseja alterar para a pasta pai. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="remarks"></a>Comentários

Se as extensões de comando estiverem habilitadas, as seguintes condições se aplicarão ao comando **CD** :

- A cadeia de caracteres do diretório atual é convertida para usar o mesmo caso que os nomes no disco. Por exemplo, `cd c:\temp` definiria o diretório atual como C:\temp se esse for o caso no disco.

- Os espaços não são tratados como delimitadores `<path>` , portanto, podem conter espaços sem aspas delimitados. Por exemplo: 

  ```
  cd username\programs\start menu
  ```

  é igual a:  
  
  ```
  cd "username\programs\start menu"
  ```

  Se as extensões estiverem desabilitadas, as aspas serão necessárias.

- Para desabilitar as extensões de comando, digite:

  ```
  cmd /e:off
  ```

## <a name="examples"></a>Exemplos

Para retornar ao diretório raiz, a parte superior da hierarquia de diretório de uma unidade:

```
cd\
```

Para alterar o diretório padrão em uma unidade diferente da que você está usando:

```
cd [<drive>:[<directory>]]
```

Para verificar a alteração no diretório, digite:

```
cd [<drive>:]
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando chdir](chdir.md)
