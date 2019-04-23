---
title: nslookup ls
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f15f06fe-67e7-41a9-93b5-192ab14ab380
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 632d25e29c09d7a164668128196964d082e160c3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848517"
---
# <a name="nslookup-ls"></a>nslookup ls

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Lista informações para um domínio do sistema de nome de domínio (DNS).
## <a name="syntax"></a>Sintaxe
```
ls [<Option>] <DNSDomain> [{[>] <FileName>|[>>] <FileName>}]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|<Option>|A tabela a seguir lista as opções válidas.<br /><br />--t: lista todos os registros do tipo especificado. Para obter uma descrição <querytype>, consulte **setquerytype** nas referências adicionais.<br />--r: lista de aliases de computadores no domínio DNS. Esse parâmetro é um sinônimo de **- t CNAME**<br />--unidade d: lista todos os registros para o domínio DNS. Esse parâmetro é um sinônimo de **- t ANY**<br />--h: lista as informações de CPU e sistema operacional para o domínio DNS. Esse parâmetro é um sinônimo de **- t HINFO**<br />--s: lista de serviços bem conhecidos de computadores no domínio DNS. Esse parâmetro é um sinônimo de **-t WKS**.|
|<DNSDomain>|Especifica o domínio DNS para o qual você deseja informações.|
|<FileName>|Especifica um nome de arquivo no qual salvar a saída. Você pode usar o maior que (>) e double maior que (>>) caracteres para redirecionar a saída da maneira usual.|
|{Ajuda &#124; ?}|Exibe um resumo breve dos **nslookup** subcomandos.|
## <a name="remarks"></a>Comentários
-   A saída padrão contém nomes de computador e seu IP endereços. Quando a saída é direcionada para um arquivo, hash são impressas para cada 50 registros recebidos do servidor
## <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[nslookup definir querytype](nslookup-set-querytype.md)
