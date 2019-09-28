---
title: bitsadmin addfilewithranges
description: Tópico de comandos do Windows para **Bitsadmin addfilewithranges** – adiciona um arquivo ao trabalho especificado. O BITS baixa os intervalos especificados do arquivo remoto.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 557f19f6e106e5fb73b3a229090eecf0fc048758
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382024"
---
# <a name="bitsadmin-addfilewithranges"></a>bitsadmin addfilewithranges

Adiciona um arquivo ao trabalho especificado. O BITS baixa os intervalos especificados do arquivo remoto. Essa opção é válida somente para trabalhos de download.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /AddFileWithRanges <Job> <RemoteURL> <LocalName> <RangeList>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|
|RemoteURL|*RemoteURL* é a URL do arquivo no servidor.|
|LocalName|*LocalName* é o nome do arquivo no computador local. *LocalName* deve conter um caminho absoluto para o arquivo.|
|Intervalo|*Rangelist* é uma lista delimitada por vírgula de pares de deslocamento: comprimento. Use dois pontos para separar o valor de deslocamento do valor de comprimento. Por exemplo, um valor de `0:100,2000:100,5000:eof` informa ao BITS para transferir 100 bytes do deslocamento 0, 100 bytes do deslocamento 2000 e os bytes restantes do deslocamento 5000 até o final do arquivo.|

## <a name="more-information"></a>Mais Informações

-   O token **EOF** é um valor de comprimento válido dentro dos pares de deslocamento e comprimento na *> \<RangeList*. Ele instrui o serviço a ler até o final do arquivo especificado.
-   Observe que AddFileWithRanges falhará com o código de erro 0x8020002c quando um intervalo de comprimento zero for especificado junto com outro intervalo com o mesmo deslocamento, como: C:\bits > Bitsadmin/addfilewithranges J2 http://bitsdc/dload/1k.zip c:\1k.zip 100:0100:5

    Mensagem de erro: Não é possível adicionar o arquivo ao trabalho-0x8020002c. A lista de intervalos de bytes contém alguns intervalos sobrepostos, que não têm suporte.

    Solução alternativa: não especifique o intervalo de comprimento zero primeiro. Por exemplo: Bitsadmin/addfilewithranges J2 http://bitsdc/dload/1k.zip c:\1k.zip 100:5100:0.

## <a name="examples"></a>Exemplos

O exemplo a seguir informa ao BITS para transferir 100 bytes do deslocamento 0, 100 bytes do deslocamento 2000 e os bytes restantes do deslocamento 5000 até o final do arquivo.

```
C:\>bitsadmin /addfilewithranges http://downloadsrv/10mb.zip c:\10mb.zip "0:100,2000:100,5000:eof"
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)