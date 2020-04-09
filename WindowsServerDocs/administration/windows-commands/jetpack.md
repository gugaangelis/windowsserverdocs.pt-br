---
title: jetpack
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 82a2b7ef-0db5-4575-a028-8acb0bf6c7ba
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 008e9dd4d41fe270d775b1c44d799dd16429046f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841969"
---
# <a name="jetpack"></a>jetpack

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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

## <a name="examples"></a><a name=BKMK_Examples></a>Disso
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
