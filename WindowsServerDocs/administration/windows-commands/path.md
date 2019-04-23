---
title: path
description: Saiba como definir a variável de ambiente PATH.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1bfa1349-e79a-472b-a9e6-d7a91149ae8f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 65ccaf23b0e19319383952f3a1ca436aaf4d06fd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856457"
---
# <a name="path"></a>path



Define o caminho de comando na variável de ambiente PATH (o conjunto de pastas usadas para pesquisar arquivos executáveis). Se usado sem parâmetros, **caminho** exibe o caminho de comando atual.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
path [[<Drive>:]<Path>[;...][;%PATH%]]
path ;
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[\<Drive>:]<Path>|Especifica a unidade e diretório para definir o caminho de comando.|
|;|Separa os diretórios no caminho de comando. Se usado sem outros parâmetros, **;** limpa os caminhos de comando existente da variável de ambiente PATH e direciona Cmd.exe para pesquisar somente no diretório atual.|
|% PATH %|Acrescenta o caminho de comando para o conjunto existente de diretórios listados na variável de ambiente PATH.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Quando você inclui **% PATH %** na sintaxe, Cmd.exe substitui-lo com os valores de caminho de comando encontrados na variável de ambiente PATH, eliminando a necessidade de inserir manualmente esses valores no prompt de comando.
-   O diretório atual é pesquisado sempre antes dos diretórios especificados no caminho de comando.
-   Você pode ter arquivos em um diretório que compartilham o mesmo nome mas extensões diferentes. Por exemplo, você pode ter um arquivo conta.com que inicia um programa de contabilidade e outro arquivo de conta que conecta seu servidor para a rede do sistema de contas.

    O sistema operacional Windows procura por um arquivo usando as extensões de nome de arquivo padrão na seguinte ordem de precedência: .exe,. com,. bat, e. cmd. Para executar a conta quando conta.com existir no mesmo diretório, você deve incluir a extensão. bat no prompt de comando.
-   Se dois ou mais arquivos no caminho de comando têm o mesmo nome de arquivo e extensão, **caminho** primeiro pesquisa o arquivo especificado nome no diretório atual. Em seguida, ele pesquisa os diretórios no caminho de comando na ordem em que estão listados na variável de ambiente PATH.
-   Se você colocar o **caminho** de comando em seu arquivo Autoexec, o sistema operacional Windows automaticamente anexa o caminho de pesquisa de subsistema do MS-DOS especificado toda vez que você faça logon no seu computador. Cmd.exe não usa o arquivo Autoexec. Quando iniciado a partir de um atalho, Cmd.exe herda as variáveis de ambiente definidas em Meu computador/propriedades/avançado/ambiente.

## <a name="BKMK_examples"></a>Exemplos

Para pesquisar os caminhos C:\User\Taxes, B:\User\Invest e B:\Bin para comandos externos, digite:

`path c:\user\taxes;b:\user\invest;b:\bin`

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)