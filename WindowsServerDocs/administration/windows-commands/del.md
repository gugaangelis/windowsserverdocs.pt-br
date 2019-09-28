---
title: del
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 346eede2-2085-44f5-9936-6877b5d5a833
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6e569443a56646862c7a2c9fbd2c599cede941a1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378697"
---
# <a name="del"></a>del



Exclui um ou mais arquivos. Esse comando é o mesmo que o comando **Erase** .

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
del [/p] [/f] [/s] [/q] [/a[:]<Attributes>] <Names>
erase [/p] [/f] [/s] [/q] [/a[:]<Attributes>] <Names>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Names >|Especifica uma lista de um ou mais arquivos ou diretórios. Caracteres curinga podem ser usados para excluir vários arquivos. Se um diretório for especificado, todos os arquivos dentro do diretório serão excluídos.|
|/p|Solicita confirmação antes de excluir o arquivo especificado.|
|/f|Força a exclusão de arquivos somente leitura.|
|/s|Exclui os arquivos especificados do diretório atual e de todos os subdiretórios. Exibe os nomes dos arquivos à medida que eles são excluídos.|
|/q|Especifica o modo silencioso. Não será solicitada a confirmação de exclusão.|
|/a [:] \<Attributes >|Exclui arquivos com base nos seguintes atributos de arquivo:</br>arquivos somente leitura do **r**</br>arquivos ocultos **h**</br>Não **tenho** conteúdo de arquivos indexados</br>**s** arquivos do sistema</br>**um** arquivo pronto para arquivamento</br>**l** pontos de nova análise</br>-Prefix significado ' not '|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

> [!CAUTION]
> Se você usar **del** para excluir um arquivo do disco, não poderá recuperá-lo.
> -   Se você usar **/p**, **del** exibirá o nome de um arquivo e enviará a seguinte mensagem:

    `FileName, Delete (Y/N)?`

    To confirm the deletion, press Y. To cancel the deletion and display the next file name (that is, if you specified a group of files), press N. To stop the **del** command, press CTRL+C.
- Se você desabilitar as extensões de comando, **/s** exibirá os nomes dos arquivos que não foram encontrados, em vez de exibir os nomes dos arquivos que estão sendo excluídos (ou seja, o comportamento será invertido).
- Se você especificar uma pasta em *nomes*, todos os arquivos na pasta serão excluídos. Por exemplo, o comando a seguir exclui todos os arquivos na pasta \Work:  
  ```
  del \work
  ```  
- Você pode usar caracteres curinga ( **&#42;** e **?** ) para excluir mais de um arquivo de cada vez. No entanto, para evitar a exclusão acidental de arquivos, você deve usar caracteres curinga com cuidado com o comando **del** . Por exemplo, se você digitar o seguinte comando:  
  ```
  del *.*
  ```  
  O comando **del** exibe o seguinte prompt:

  `Are you sure (Y/N)?`

  Para excluir todos os arquivos no diretório atual, pressione Y e pressione ENTER. Para cancelar a exclusão, pressione N e pressione ENTER.

> [!NOTE]
> Antes de usar caracteres curinga com o comando **del** , use os mesmos caracteres curinga com o comando **dir** para listar todos os arquivos que serão excluídos.
> -   O comando **del** , com parâmetros diferentes, está disponível no console de recuperação.

## <a name="BKMK_examples"></a>Disso

Para excluir todos os arquivos em uma pasta chamada Test na unidade C, digite um dos seguintes:
```
del c:\test
del c:\test\*.*
```
Para excluir todos os arquivos com a extensão de nome de arquivo. bat do diretório atual, digite:
```
del *.bat
```
Para excluir todos os arquivos somente leitura no diretório atual, digite:
```
del /a:r *.*
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
