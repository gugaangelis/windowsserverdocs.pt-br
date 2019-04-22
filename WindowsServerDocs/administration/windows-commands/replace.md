---
title: replace
description: Saiba como usar o comando replace para substituir os arquivos.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6143661e-d90f-4812-b265-6669b567dd1f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: bce4622983271bde06614ebc04418871490cff91
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818887"
---
# <a name="replace"></a>replace



Substitui os arquivos. Se usado com o **/a** opção **substituir** adiciona novos arquivos para um diretório em vez de substituir arquivos existentes.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
replace [<Drive1>:][<Path1>]<FileName> [<Drive2>:][<Path2>] [/a] [/p] [/r] [/w] 
replace [<Drive1>:][<Path1>]<FileName> [<Drive2>:][<Path2>] [/p] [/r] [/s] [/w] [/u] 
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[\<Drive1>:][\<Path1>]\<FileName>|Especifica o local e o nome do arquivo de origem ou o conjunto de arquivos. *Nome do arquivo* é necessário e pode incluir caracteres curinga (**&#42;** e **?**).|
|[\<Drive2>:][\<Path2>]|Especifica o local do arquivo de destino. Você não pode especificar um nome de arquivo para arquivos que você substitui. Se você não especificar uma unidade ou caminho, **substituir** usa a unidade atual e o diretório como o destino.|
|/a|Adiciona novos arquivos para o diretório de destino em vez de substituir arquivos existentes. Você não pode usar essa opção de linha de comando com o **/s** ou **/u** opção de linha de comando.|
|/p|Solicita sua confirmação antes de substituir um arquivo de destino ou a adição de um arquivo de origem.|
|/r|Substitui os arquivos desprotegidos e somente leitura. Se você tentar substituir um arquivo somente leitura, mas você não especificar **/r**, os resultados de um erro e interrompe a operação de substituição.|
|/w|Aguarda a inserção de um disco antes de começa a pesquisa de arquivos de origem. Se você não especificar **/w**, **substituir** começa a substituir ou adicionar arquivos imediatamente depois de pressionar ENTER.|
|/s|Pesquisa todos os subdiretórios no diretório de destino e substitui arquivos correspondentes. Não é possível usar **/s** com o **/a** opção de linha de comando. O **substituir** comando não pesquisa subdiretórios que são especificados na *Path1*.|
|/u|Substitui somente os arquivos mais antigos do que aqueles no diretório de origem no diretório de destino. Não é possível usar **/u** com o **/a** opção de linha de comando.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Como **substituir** adiciona ou substitui arquivos, o arquivo de nomes são exibidos na tela. Após **substituir** for concluído, uma linha de resumo é exibida em um dos seguintes formatos:  
    ```
    nnn files added
    nnn files replaced
    no file added
    no file replaced
    ```  
-   Se você estiver usando disquetes e precisar alternar discos durante o **substituir** operação, você pode especificar o **/w** opção de linha de comando, de modo que **substituir** irá esperar para que você Alterne os discos.
-   Não é possível usar **substituir** para atualizar arquivos ocultos ou arquivos de sistema.
-   A tabela a seguir mostra cada código de saída e uma breve descrição de seu significado:  
    |Código de Saída|Descrição|
    |---------|-----------|
    |0|O **substituir** comando substituído ou os arquivos foram adicionados com êxito.|
    |1|O **substituir** comando encontrou uma versão incorreta do MS-DOS.|
    |2|O **substituir** comando não foi possível localizar os arquivos de origem.|
    |3|O **substituir** comando não foi possível encontrar o caminho de origem ou destino.|
    |5|O usuário não tem acesso aos arquivos que você deseja substituir.|
    |8|Não há memória de sistema insuficiente para executar o comando.|
    |11|O usuário utilizou a sintaxe incorreta na linha de comando.|

> [!NOTE]
> Você pode usar o parâmetro ERRORLEVEL sobre o **se** linha de comando em um arquivo em lotes para processar códigos de saída que são retornados pelo **substituir**.

## <a name="BKMK_examples"></a>Exemplos

Para atualizar todas as versões de um arquivo denominado CLI (que aparecem em vários diretórios na unidade C), com a versão mais recente do arquivo CLI de um disquete na unidade A, digite:

`replace a:\phones.cli c:\ /s`

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)