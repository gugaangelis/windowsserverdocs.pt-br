---
title: bitsadmin addfilewithranges
description: Tópico de comandos do Windows para **addfilewithranges bitsadmin** -adiciona um arquivo para o trabalho especificado. Downloads do BITS dos intervalos especificados do arquivo remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: df0ce0bf-dff1-4a48-a16f-fd2f4d5f7189
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d5e1e4f8af9117928f9ab044d29e65f57aa5a119
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811275"
---
# <a name="bitsadmin-addfilewithranges"></a>bitsadmin addfilewithranges

Adiciona um arquivo para o trabalho especificado. Downloads do BITS dos intervalos especificados do arquivo remoto. Essa opção é válida somente para trabalhos de download.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /AddFileWithRanges <Job> <RemoteURL> <LocalName> <RangeList>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|
|RemoteURL|*RemoteURL* é a URL do arquivo no servidor.|
|LocalName|*LocalName* é o nome do arquivo no computador local. *LocalName* deve conter um caminho absoluto para o arquivo.|
|RangeList|*RangeList* é uma lista delimitada por vírgula de pares de deslocamento: comprimento. Use uma vírgula para separar o valor de deslocamento do valor de comprimento. Por exemplo, um valor de `0:100,2000:100,5000:eof` informa o BITS para transferir 100 bytes do deslocamento de 0, 100 bytes do deslocamento de 2000 e os bytes restantes de deslocamento 5000 até o final do arquivo.|

## <a name="more-information"></a>Mais Informações

-   O token **eof** é um valor de comprimento válido dentro de pares de deslocamento e comprimento na  *\<RangeList >* . Ele instrui o serviço para ler até o final do arquivo especificado.
-   Observe que o AddFileWithRanges falhará com o código de erro 0x8020002c quando um intervalo de comprimento zero for especificado, juntamente com outro intervalo com o mesmo deslocamento, tais como: C:\bits > bitsadmin /addfilewithranges j2 http://bitsdc/dload/1k.zip c:\1k.zip 100:0, 100:5

    Mensagem de erro: Não é possível adicionar arquivo ao trabalho - 0x8020002c. A lista de intervalos de bytes contém alguns intervalos sobrepostos, que não têm suporte.

    Solução alternativa: não especifique o intervalo de comprimento zero pela primeira vez. Por exemplo: bitsadmin /addfilewithranges j2 http://bitsdc/dload/1k.zip c:\1k.zip 100:5, 100:0.

## <a name="examples"></a>Exemplos

O exemplo a seguir informa o BITS para transferir 100 bytes do deslocamento de 0, 100 bytes do deslocamento 2000 e os bytes restantes de deslocamento 5000 até o final do arquivo.

```
C:\>bitsadmin /addfilewithranges http://downloadsrv/10mb.zip c:\10mb.zip "0:100,2000:100,5000:eof"
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)