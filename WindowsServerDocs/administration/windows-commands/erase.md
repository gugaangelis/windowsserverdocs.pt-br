---
title: erase
description: Tópico de referência para o comando Erase, que exclui um ou mais arquivos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 024a4d0f-8679-4e06-b46f-61fdaf5464bc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1525f20549b6571975cb622534f8504eb4fa1f79
ms.sourcegitcommit: 5e313a004663adb54c90962cfdad9ae889246151
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84354676"
---
# <a name="erase"></a>erase

Exclui um ou mais arquivos. Se você usar **apagar** para excluir um arquivo do disco, não poderá recuperá-lo.

> [!NOTE]
> Esse comando é o mesmo que o [comando del](del.md).


## <a name="syntax"></a>Sintaxe

```
erase [/p] [/f] [/s] [/q] [/a[:]<attributes>] <names>
del [/p] [/f] [/s] [/q] [/a[:]<attributes>] <names>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<names>` | Especifica uma lista de um ou mais arquivos ou diretórios. Caracteres curinga podem ser usados para excluir vários arquivos. Se um diretório for especificado, todos os arquivos dentro do diretório serão excluídos. |
| /p | Solicita confirmação antes de excluir o arquivo especificado. |
| /f | Força a exclusão de arquivos somente leitura. |
| /s | Exclui os arquivos especificados do diretório atual e de todos os subdiretórios. Exibe os nomes dos arquivos à medida que eles são excluídos. |
| /q | Especifica o modo silencioso. Não será solicitada a confirmação de exclusão. |
| /a [:]`<attributes>` | Exclui arquivos com base nos seguintes atributos de arquivo:<ul><li>arquivos somente leitura do **r**</li><li>arquivos ocultos **h**</li><li>Não **tenho** conteúdo de arquivos indexados</li><li>**s** arquivos do sistema</li><li>**um** arquivo pronto para arquivamento</li><li>**l** pontos de nova análise</li><li>**-** Usado como um prefixo que significa ' not '</li></ul>. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Se você usar o `erase /p` comando, verá a seguinte mensagem:

    `FileName, Delete (Y/N)?`

    Para confirmar a exclusão, pressione **Y**. Para cancelar a exclusão e exibir o próximo nome de arquivo (se você especificou um grupo de arquivos), pressione **N**. Para interromper o comando **Erase** , pressione CTRL + C.

- Se você desabilitar a extensão de comando, o parâmetro **/s** exibirá os nomes de todos os arquivos que não foram encontrados, em vez de exibir os nomes dos arquivos que estão sendo excluídos.

- Se você especificar pastas específicas no `<names>` parâmetro, todos os arquivos incluídos também serão excluídos. Por exemplo, se você quiser excluir todos os arquivos na pasta *\Work* , digite:

  ```
  erase \work
  ```

- Você pode usar caracteres curinga (**&#42;** e **?**) para excluir mais de um arquivo por vez. No entanto, para evitar a exclusão acidental de arquivos, você deve usar caracteres curinga com cuidado. Por exemplo, se você digitar o seguinte comando:

  ```
  erase *.*
  ```

  O comando **Erase** exibe o seguinte prompt:

  `Are you sure (Y/N)?`

  Para excluir todos os arquivos no diretório atual, pressione **Y** e pressione Enter. Para cancelar a exclusão, pressione **N** e pressione Enter.

  > [!NOTE]
  > Antes de usar caracteres curinga com o comando **Erase** , use os mesmos caracteres curinga com o comando **dir** para listar todos os arquivos que serão excluídos.

### <a name="examples"></a>Exemplos

Para excluir todos os arquivos em uma pasta chamada Test na unidade C, digite um dos seguintes:

```
erase c:\test
erase c:\test\*.*
```

Para excluir todos os arquivos com a extensão de nome de arquivo. bat do diretório atual, digite:

```
erase *.bat
```

Para excluir todos os arquivos somente leitura no diretório atual, digite:

```
erase /a:r *.*
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando del](del.md)