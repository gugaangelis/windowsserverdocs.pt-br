---
title: jetpack
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82a2b7ef-0db5-4575-a028-8acb0bf6c7ba
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a3bffc29519df139921bdb1de53e67acd558b306
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858007"
---
# <a name="jetpack"></a>jetpack

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

compacta um banco de dados do serviço de nome na Internet do Windows (WINS) ou protocolo de configuração de Host dinâmico (DHCP). A Microsoft recomenda que você compacte o banco de dados do WINS sempre que ele se aproxima de 30 MB. 

## <a name="syntax"></a>Sintaxe
```
jetpack.EXE <database name> <temp database name>
```

### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|<database name>|Especifica o arquivo de banco de dados original.|
|<temp database name>|Especifica o arquivo de banco de dados temporário.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="BKMK_Examples"></a>Exemplos
Para compactar o banco de dados do WINS:
```
cd %SYSTEMROOT%\SYSTEM32\WINS
NET STOP WINS
jetpack WINS.MDB TMP.MDB
NET start WINS
```
Para compactar um banco de dados DHCP:
```
cd %SYSTEMROOT%\SYSTEM32\DHCP
NET STOP DHCPSERver
jetpack DHCP.MDB TMP.MDB
NET start DHCPSERver
```
Nos exemplos acima, **TMP** é um banco de dados temporário que é usado pelo jetpack.exe. **MDB** é o banco de dados do WINS. **MDB** é o banco de dados DHCP.
Jetpack.exe compacta o WINS ou o banco de dados DHCP, fazendo o seguinte:
1.  Informações para um arquivo de banco de dados temporário chamado de banco de dados de cópias **TMP**.
2.  Exclui o arquivo de banco de dados original, **mdb** ou **mdb**.
3.  Renomeia os arquivos de banco de dados temporário para o nome do arquivo original.

> [!NOTE]
> Durante o processo de compactação, jetpack.exe cria um arquivo temporário com o nome especificado, o *nome do banco de dados temporário* parâmetro. O arquivo temporário é removido quando o processo de compactação for concluído. Verifique se você não tem um arquivo já existente no WINS ou DHCP pasta com o mesmo nome que aquele especificado na *nome do banco de dados temporário* parâmetro.

## <a name="additional-references"></a>Referências adicionais
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
