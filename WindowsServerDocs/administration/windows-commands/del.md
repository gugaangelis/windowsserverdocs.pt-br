---
title: del
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: a1c2756e53d9f047160ddd037b3868e47d6e3181
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822987"
---
# <a name="del"></a>del



Exclui um ou mais arquivos. Esse comando é igual a **apagar** comando.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
del [/p] [/f] [/s] [/q] [/a[:]<Attributes>] <Names>
erase [/p] [/f] [/s] [/q] [/a[:]<Attributes>] <Names>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Nomes >|Especifica uma lista de um ou mais arquivos ou diretórios. Caracteres curinga pode ser usados para excluir vários arquivos. Se for especificado um diretório, todos os arquivos dentro do diretório serão excluídos.|
|/p|Solicita confirmação antes de excluir o arquivo especificado.|
|/f|Força a exclusão de arquivos somente leitura.|
|/s|Exclui os arquivos do diretório atual e todos os subdiretórios especificados. Exibe os nomes dos arquivos como eles estão sendo excluídos.|
|/q|Especifica o modo silencioso. Você não será solicitado a confirmar a exclusão.|
|/a[:]\<Attributes>|Exclui arquivos com base em atributos de arquivo a seguir:</br>**r** arquivos somente leitura</br>**h** arquivos ocultos</br>**Eu** não indexadas de arquivos de conteúdo</br>**s** arquivos do sistema</br>**um** arquivos prontos para arquivamento</br>**l** pontos de nova análise</br>-Ou seja, 'not' de prefixo|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

> [!CAUTION]
> Se você usar **/DEL** para excluir um arquivo do disco, você não pode recuperá-lo.
-   Se você usar **/p**, **/DEL** exibe o nome de um arquivo e envia a mensagem a seguir:

    `FileName, Delete (Y/N)?`

    Para confirmar a exclusão, pressione Y. Para cancelar a exclusão e exibir o próximo nome de arquivo (ou seja, se você especificou um grupo de arquivos), pressione N. Para parar o **/DEL** de comando, pressione CTRL + C.
-   Se você desabilitar as extensões de comando **/s** exibe os nomes de todos os arquivos que não foram encontrados em vez de exibir os nomes dos arquivos que estão sendo excluídos (ou seja, o comportamento é invertido).
-   Se você especificar uma pasta no *nomes*, todos os arquivos na pasta são excluídos. Por exemplo, o comando a seguir exclui todos os arquivos na pasta \Work:  
    ```
    del \work
    ```  
-   Você pode usar caracteres curinga (**&#42;** e **?**) para excluir mais de um arquivo por vez. No entanto, para evitar a exclusão de arquivos acidentalmente, você deve usar caracteres curinga com cuidado com o **/DEL** comando. Por exemplo, se você digitar o comando a seguir:  
    ```
    del *.*
    ```  
    O **/DEL** comando exibe o prompt a seguir:

    `Are you sure (Y/N)?`

    Para excluir todos os arquivos no diretório atual, pressione Y e pressione ENTER. Para cancelar a exclusão, pressione N e pressione ENTER.

> [!NOTE]
> Antes de usar caracteres curinga com o **/DEL** de comando, use os mesmos caracteres curinga com o **dir** comando para listar todos os arquivos que serão excluídos.
-   O **/DEL** comando com parâmetros diferentes, está disponível no Console de recuperação.

## <a name="BKMK_examples"></a>Exemplos

Para excluir todos os arquivos em uma pasta chamada teste na unidade C, digite o seguinte:
```
del c:\test
del c:\test\*.*
```
Para excluir todos os arquivos com a extensão de nome de arquivo. bat do diretório atual, digite:
```
del *.bak
```
Para excluir todos os arquivos somente leitura no diretório atual, digite:
```
del /a:r *.*
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)