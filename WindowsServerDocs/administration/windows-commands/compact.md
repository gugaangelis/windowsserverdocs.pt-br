---
title: compact
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 429b3752-df0a-43a4-a210-df2f3ad03c3b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 068111b293a3eb3987b14744a1bfcf2fde26bced
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826027"
---
# <a name="compact"></a>compact



Exibe ou altera a compactação de arquivos ou diretórios em partições NTFS. Se usado sem parâmetros, **compact** exibe o estado de compactação do diretório atual e os arquivos que ele contém.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
compact [/c | /u] [/s[:<Dir>]] [/a] [/i] [/f] [/q] [<FileName>[...]]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/c|Compacta o arquivo ou diretório especificado.|
|/u|Descompacta o arquivo ou diretório especificado.|
|/s[:\<Dir>]|Aplica-se a **compact** comando para todos os subdiretórios do diretório especificado (ou do diretório atual se nenhum for especificado).|
|/a|Exibe ocultada ou arquivos do sistema.|
|/i|Ignora erros.|
|/f|Força a compactação ou descompactação de arquivo ou diretório especificado. **/f** é usado no caso de um arquivo que foi compactado parcialmente quando a operação foi interrompida por uma falha do sistema. Para forçar o arquivo deve ser compactado em sua totalidade, use o **/c** e **/f** parâmetros e especifique o arquivo compactado parcialmente.|
|/q|Relata somente as informações essenciais.|
|\<FileName>|Especifica o arquivo ou diretório. Você pode usar vários nomes de arquivo e o **&#42;** e **?** caracteres curinga.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   O **compact** comando é a versão de linha de comando do recurso de compactação do sistema de arquivos NTFS. O estado de compactação de um diretório indica se os arquivos são compactados automaticamente quando eles são adicionados ao diretório. Definindo o estado de compactação de um diretório não altera o estado de compactação de arquivos que já estão no diretório necessariamente.
-   Não é possível usar **compactar** para leitura, gravação ou volumes de montagem que foram compactados usando DriveSpace ou DoubleSpace.
-   Não é possível usar **compactar** para compactar a tabela de alocação de arquivos (FAT) ou partições FAT32.

## <a name="BKMK_examples"></a>Exemplos

Para definir o estado de compactação do diretório atual, seus subdiretórios e arquivos existentes, digite:
```
compact /c /s 
```
Para definir o estado de compactação de arquivos e subdiretórios do diretório atual, sem alterar o estado de compactação do diretório atual em si, digite:
```
compact /c /s *.*
```
Para compactar um volume, no diretório raiz do volume, digite:
```
compact /c /i /s:\
```

> [!NOTE]
> Este exemplo define o estado de compactação de todos os diretórios (incluindo o diretório raiz do volume) e compacta todos os arquivos no volume. O **/i** parâmetro impede que mensagens de erro interromper o processo de compactação.

Para compactar todos os arquivos com a extensão de nome de arquivo. bmp na pasta \Tmp e todos os subdiretórios do \Tmp, sem modificar o atributo compactado dos diretórios, digite:
```
compact /c /s:\tmp *.bmp
```
Para forçar a compactação completa do arquivo Zebra, que foi compactado parcialmente durante uma falha do sistema, digite:
```
compact /c /f zebra.bmp
```
Para remover o atributo compactado da pasta C:\Tmp, sem alterar o estado de compactação de todos os arquivos nesse diretório, digite:
```
compact /u c:\tmp
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)