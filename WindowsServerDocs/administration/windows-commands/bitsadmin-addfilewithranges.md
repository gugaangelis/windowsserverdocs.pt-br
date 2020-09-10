---
title: bitsadmin addfilewithranges
description: Artigo de referência para o comando Bitsadmin addfilewithranges, que adiciona um arquivo ao trabalho especificado. O BITS baixa os intervalos especificados do arquivo remoto.
ms.topic: reference
ms.assetid: df0ce0bf-dff1-4a48-a16f-fd2f4d5f7189
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 5eda860558f43b5f692bd41167617b24dbb6f86b
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632649"
---
# <a name="bitsadmin-addfilewithranges"></a>bitsadmin addfilewithranges

Adiciona um arquivo ao trabalho especificado. O BITS baixa os intervalos especificados do arquivo remoto. Essa opção é válida somente para trabalhos de download.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /addfilewithranges <job> <remoteURL> <localname> <rangelist>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| remoteURL | URL do arquivo no servidor. |
| localname | Nome do arquivo no computador local. Deve conter um caminho absoluto para o arquivo. |
| intervalo | Lista delimitada por vírgulas de deslocamento: pares de comprimento. Use dois pontos para separar o valor de deslocamento do valor de comprimento. Por exemplo, um valor `0:100,2000:100,5000:eof` informa ao bits para transferir 100 bytes do deslocamento 0, 100 bytes do deslocamento 2000 e os bytes restantes do deslocamento 5000 até o final do arquivo. |

## <a name="remarks"></a>Comentários

- O token **EOF** é um valor de comprimento válido dentro dos pares de deslocamento e comprimento no `<rangelist>` . Ele instrui o serviço a ler até o final do arquivo especificado.

- O `addfilewithranges` comando falhará com o código de erro 0x8020002c, se um intervalo de comprimento zero for especificado junto com outro intervalo usando o mesmo deslocamento, como:

    `c:\bits>bitsadmin /addfilewithranges j2 http://bitsdc/dload/1k.zip c:\1k.zip 100:0,100:5`

    **Mensagem de erro:** Não é possível adicionar o arquivo ao trabalho-0x8020002c. A lista de intervalos de bytes contém alguns intervalos sobrepostos, que não têm suporte.

    **Solução alternativa:** Não especifique o intervalo de comprimento zero primeiro. Por exemplo, use `bitsadmin /addfilewithranges j2 http://bitsdc/dload/1k.zip c:\1k.zip 100:5,100:0`

## <a name="examples"></a>Exemplos

Para transferir 100 bytes do deslocamento 0, 100 bytes do deslocamento 2000 e os bytes restantes do deslocamento 5000 para o final do arquivo:

```
bitsadmin /addfilewithranges http://downloadsrv/10mb.zip c:\10mb.zip 0:100,2000:100,5000:eof
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
