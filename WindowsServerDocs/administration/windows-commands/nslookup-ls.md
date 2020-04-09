---
title: nslookup ls
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f15f06fe-67e7-41a9-93b5-192ab14ab380
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dbaef500410d6427beb008109abcc2c339b8d65c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838729"
---
# <a name="nslookup-ls"></a>nslookup ls

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Lista informações para um domínio DNS (sistema de nomes de domínio).
## <a name="syntax"></a>Sintaxe
```
ls [<Option>] <DNSDomain> [{[>] <FileName>|[>>] <FileName>}]
```
### <a name="parameters"></a>Parâmetros

|    Parâmetro    |                                                                                                                                                                                                                                                                                                               Descrição                                                                                                                                                                                                                                                                                                                |
|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <Option>     | A tabela a seguir lista as opções válidas.<p>--t: lista todos os registros do tipo especificado. Para obter uma descrição de <querytype>, consulte **setconsultatype** em referências adicionais.<br />--a: lista aliases de computadores no domínio DNS. Este parâmetro é um sinônimo para **-t CNAME**<br />--d: lista todos os registros para o domínio DNS. Esse parâmetro é um sinônimo para **-t**<br />--h: lista informações de CPU e do sistema operacional para o domínio DNS. Este parâmetro é um sinônimo para **-t HINFO**<br />--s: lista os serviços bem conhecidos de computadores no domínio DNS. Esse parâmetro é um sinônimo para **-t WKS**. |
|   <DNSDomain>   |                                                                                                                                                                                                                                                                                         Especifica o domínio DNS para o qual você deseja informações.                                                                                                                                                                                                                                                                                         |
|   <FileName>    |                                                                                                                                                                                                                                 Especifica um nome de arquivo no qual salvar a saída. Você pode usar os caracteres maior que (>) e duplo maior que (> >) para redirecionar a saída da maneira usual.                                                                                                                                                                                                                                  |
| {ajuda &#124; ?} |                                                                                                                                                                                                                                                                                          Exibe um breve resumo dos subcomandos **nslookup** .                                                                                                                                                                                                                                                                                           |

## <a name="remarks"></a>Comentários
- A saída padrão contém nomes de computador e seus endereços IP. Quando a saída é direcionada para um arquivo, as marcas de hash são impressas para cada 50 registros recebidos do servidor
  ## <a name="additional-references"></a>Referências adicionais
  - [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
  [nslookup Set QueryType](nslookup-set-querytype.md)
