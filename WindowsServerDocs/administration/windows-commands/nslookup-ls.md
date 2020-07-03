---
title: nslookup ls
description: Artigo de referência para o comando nslookup ls, que lista informações de domínio DNS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f15f06fe-67e7-41a9-93b5-192ab14ab380
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7c0c0aeb01cdffb1f2b779178f0298e1f120348e
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85936568"
---
# <a name="nslookup-ls"></a>nslookup ls

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Lista informações de domínio DNS.

## <a name="syntax"></a>Sintaxe

```
ls [<option>] <DNSdomain> [{[>] <filename>|[>>] <filename>}]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<option>` | As opções válidas incluem:<ul><li>**-t:** Lista todos os registros do tipo especificado. Para obter mais informações, consulte [nslookup Set QueryType](nslookup-set-querytype.md).</li><li>**-a:** Lista aliases de computadores no domínio DNS. Esse parâmetro é o mesmo que **-t CNAME**</li><li>**-d:** Lista todos os registros para o domínio DNS. Esse parâmetro é o mesmo que **-t qualquer**</li><li>**-h:** Lista informações de CPU e do sistema operacional para o domínio DNS. Esse parâmetro é o mesmo que **-t HINFO**</li><li>**-s:** Lista os serviços bem conhecidos de computadores no domínio DNS. Esse parâmetro é o mesmo **WKS de-t**. |
| `<DNSdomain>` | Especifica o domínio DNS para o qual você deseja informações. |
| `<filename>` | Especifica um nome de arquivo a ser usado para a saída salva. Você pode usar os caracteres maior que ( `>` ) e duplo maior que ( `>>` ) para redirecionar a saída da maneira usual. |
| /? | Exibe a ajuda no prompt de comando. |
| /help | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- A saída padrão desse comando inclui nomes de computador e seus endereços IP associados.

- Se a saída for direcionada para um arquivo, as marcas de hash serão adicionadas a cada 50 registros recebidos do servidor.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [nslookup set querytype](nslookup-set-querytype.md)
