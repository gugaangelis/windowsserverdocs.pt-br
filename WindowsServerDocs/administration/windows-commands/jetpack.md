---
title: jetpack
description: Artigo de referência para o comando jetpack, que compacta um WINS (serviço de cadastramento na Internet do Windows) ou um banco de dados DHCP (protocolo de configuração dinâmica de hosts).
ms.topic: reference
ms.assetid: 82a2b7ef-0db5-4575-a028-8acb0bf6c7ba
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bf2664c142a169982acc0d11c34a53aea193d030
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89035414"
---
# <a name="jetpack"></a>jetpack

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Compacta um WINS (serviço de cadastramento na Internet do Windows) ou um banco de dados DHCP (protocolo de configuração dinâmica de hosts). Recomendamos que você compacte o banco de dados do WINS sempre que ele se aproximar de 30 MB.

Jetpack.exe compacta o banco de dados:

1. Copiar as informações do banco de dados para um arquivo de banco de dados temporário.

2. Excluindo o arquivo de banco de dados original, WINS ou DHCP.

3. Renomeia os arquivos de banco de dados temporários para o nome de arquivo original.

## <a name="syntax"></a>Sintaxe

```
jetpack.exe <database_name> <temp_database_name>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| ------- | -------- |
| `<database_name>` | Especifica o nome do arquivo de banco de dados original. |
| `<temp_database_name>` | Especifica o nome do arquivo de banco de dados temporário a ser criado por jetpack.exe.<p>Observação: esse arquivo temporário é removido quando o processo de compactação é concluído. Para que esse comando funcione corretamente, você deve verificar se o nome do arquivo temporário é exclusivo e se um arquivo com esse nome ainda não existe. |
| /? | Exibe a ajuda no prompt de comando. |

### <a name="examples"></a>Exemplos

Para compactar o banco de dados WINS, em que **tmp. mdb** é um banco de dados temporário e o **WINS. mdb** é o banco de dados do WINS, digite:

```
cd %SYSTEMROOT%\SYSTEM32\WINS
NET STOP WINS
jetpack Wins.mdb Tmp.mdb
NET start WINS
```

Para compactar o banco de dados DHCP, em que **tmp. mdb** é um banco de dados temporário e **DHCP. mdb** é o banco de dados DHCP, digite:

```
cd %SYSTEMROOT%\SYSTEM32\DHCP
NET STOP DHCPSERVER
jetpack Dhcp.mdb Tmp.mdb
NET start DHCPSERVER
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
