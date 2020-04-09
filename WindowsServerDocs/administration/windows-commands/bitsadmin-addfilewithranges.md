---
title: bitsadmin addfilewithranges
description: Tópico de comandos do Windows para **Bitsadmin addfilewithranges**, que adiciona um arquivo ao trabalho especificado. O BITS baixa os intervalos especificados do arquivo remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: df0ce0bf-dff1-4a48-a16f-fd2f4d5f7189
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 60b448550473691bbbaf573f99f00609e14e0f42
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850949"
---
# <a name="bitsadmin-addfilewithranges"></a>bitsadmin addfilewithranges

Adiciona um arquivo ao trabalho especificado. O BITS baixa os intervalos especificados do arquivo remoto. Essa opção é válida somente para trabalhos de download.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /AddFileWithRanges <Job> <RemoteURL> <LocalName> <RangeList>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| Trabalho | O nome de exibição ou o GUID do trabalho. |
| RemoteURL | URL do arquivo no servidor. |
| LocalName | Nome do arquivo no computador local. Deve conter um caminho absoluto para o arquivo. |
| Intervalo | Lista delimitada por vírgulas de deslocamento: pares de comprimento. Use dois pontos para separar o valor de deslocamento do valor de comprimento. Por exemplo, um valor de `0:100,2000:100,5000:eof` informa ao BITS para transferir 100 bytes do deslocamento 0, 100 bytes do deslocamento 2000 e os bytes restantes do deslocamento 5000 até o final do arquivo. |

## <a name="remarks"></a>Comentários

- O token **EOF** é um valor de comprimento válido dentro dos pares de deslocamento e comprimento na *> de intervalo\<* . Ele instrui o serviço a ler até o final do arquivo especificado.

- AddFileWithRanges falhará com o código de erro 0x8020002c, quando um intervalo de comprimento zero for especificado junto com outro intervalo com o mesmo deslocamento, como:

    `C:\bits>bitsadmin /addfilewithranges j2 http://bitsdc/dload/1k.zip c:\1k.zip 100:0,100:5`

    **Mensagem de erro:** Não é possível adicionar o arquivo ao trabalho-0x8020002c. A lista de intervalos de bytes contém alguns intervalos sobrepostos, que não têm suporte.

    **Solução alternativa:** Não especifique o intervalo de comprimento zero primeiro. Por exemplo, use: 

    `bitsadmin /addfilewithranges j2 http://bitsdc/dload/1k.zip c:\1k.zip 100:5,100:0.`

## <a name="examples"></a>Exemplos

O exemplo a seguir informa ao BITS para transferir 100 bytes do deslocamento 0, 100 bytes do deslocamento 2000 e os bytes restantes do deslocamento 5000 até o final do arquivo.

```
C:\>bitsadmin /addfilewithranges http://downloadsrv/10mb.zip c:\10mb.zip 0:100,2000:100,5000:eof
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)