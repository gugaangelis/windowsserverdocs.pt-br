---
title: jetpack
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 82a2b7ef-0db5-4575-a028-8acb0bf6c7ba
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ec29a1fd48fdba72f07fe5d00de7730d93434105
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724792"
---
# <a name="jetpack"></a>jetpack

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

compacta um WINS (serviço de cadastramento na Internet do Windows) ou um banco de dados DHCP (protocolo de configuração dinâmica de hosts). A Microsoft recomenda que você compacte o banco de dados WINS sempre que ele se aproximar de 30 MB. 

## <a name="syntax"></a>Sintaxe
```
jetpack.EXE <database name> <temp database name>
```

#### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|<database name>|Especifica o arquivo de banco de dados original.|
|<temp database name>|Especifica o arquivo de banco de dados temporário.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="examples"></a>Exemplos
Para compactar o banco de dados WINS:
```
cd %SYSTEMROOT%\SYSTEM32\WINS
NET STOP WINS
jetpack WINS.MDB TMP.MDB
NET start WINS
```
Para compactar o banco de dados DHCP:
```
cd %SYSTEMROOT%\SYSTEM32\DHCP
NET STOP DHCPSERver
jetpack DHCP.MDB TMP.MDB
NET start DHCPSERver
```
Nos exemplos acima, **tmp. mdb** é um banco de dados temporário que é usado pelo jetpack. exe. **WINS. mdb** é o banco de dados WINS. **DHCP. mdb** é o banco de dados DHCP.
o Jetpack. exe compacta o banco de dados WINS ou DHCP fazendo o seguinte:
1.  Copia informações do banco de dados em um arquivo de banco de dados temporário chamado **tmp. mdb**.
2.  exclui o arquivo de banco de dados original, **WINS. mdb** ou **DHCP. mdb**.
3.  renomeia os arquivos de banco de dados temporários para o nome de arquivo original.

> [!NOTE]
> Durante o processo compacto, o Jetpack. exe cria um arquivo temporário com o nome especificado pelo parâmetro *nome do banco de dados Temp* . O arquivo temporário é removido quando o processo de compactação é concluído. Verifique se você não tem um arquivo já existente na pasta WINS ou DHCP com o mesmo nome que o especificado no parâmetro nome do *banco de dados Temp* .

## <a name="additional-references"></a>Referências adicionais
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
