---
title: importação de logman | Export
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c258daba-fb93-47c0-a53b-2fe83ed2c743
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 309274b5288bd1c17259e01cf563ae8685a2094e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374464"
---
# <a name="logman-import--export"></a>importação de logman | Export

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

importar um conjunto de coletores de dados de um arquivo XML ou exportar um conjunto de coletores de dados para um arquivo XML.  

## <a name="syntax"></a>Sintaxe  
```  
logman import <[-n] <name>> <-xml <name>> [options]  
logman export <[-n] <name>> <-xml <name>> [options]  
```  
## <a name="parameters"></a>Parâmetros  

|        Parâmetro        |                                                                        Descrição                                                                        |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
|           -?            |                                                             Exibe a ajuda contextual.                                                              |
|   -s <computer name>    |                                                   Execute o comando no computador remoto especificado.                                                   |
|     -config <value>     |                                                  Especifica o arquivo de configurações que contém as opções de comando.                                                  |
|       [-n] <name>       |                                                                Nome do objeto de destino.                                                                 |
|       -XML <name>       |                                                         Nome do arquivo XML a ser importado ou exportado.                                                         |
|          -ETS           |                                       Envie comandos para sessões de rastreamento de eventos diretamente sem salvar ou agendar.                                        |
| -[-] u < usuário [senha] > | Usuário a ser executado como. Inserir um \* para a senha produz uma solicitação para a senha. A senha não é exibida quando você a digita no prompt de senha. |
|           -y            |                                                      Responda sim a todas as perguntas sem avisar.                                                       |

## <a name="BKMK_examples"></a>Disso  
O comando a seguir importa o arquivo XML c:\windows\perf_log.XML do computador server_1 como um conjunto de coletores de dados chamado perf_log.  
```  
logman import perf_log -s server_1 -xml "c:\windows\perf_log.xml"  
```  
#### <a name="additional-references"></a>Referências adicionais  
[logman](logman.md)  
