---
title: importação de logman | Export
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c258daba-fb93-47c0-a53b-2fe83ed2c743
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ffc2e42f353352f69cf61dfb1f108a7d53cb7c4b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724368"
---
# <a name="logman-import--export"></a>importação de logman | Export

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

importar um conjunto de coletores de dados de um arquivo XML ou exportar um conjunto de coletores de dados para um arquivo XML.  

## <a name="syntax"></a>Sintaxe  
```  
logman import <[-n] <name>> <-xml <name>> [options]  
logman export <[-n] <name>> <-xml <name>> [options]  
```  
### <a name="parameters"></a>Parâmetros  

|        Parâmetro        |                                                                        Descrição                                                                        |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
|           -?            |                                                             Exibe a ajuda contextual.                                                              |
|   -s<computer name>    |                                                   Execute o comando no computador remoto especificado.                                                   |
|     -config <value>     |                                                  Especifica o arquivo de configurações que contém as opções de comando.                                                  |
|       [-n]<name>       |                                                                O nome do objeto de destino.                                                                 |
|       -XML<name>       |                                                         Nome do arquivo XML a ser importado ou exportado.                                                         |
|          -ETS           |                                       Envie comandos para sessões de rastreamento de eventos diretamente sem salvar ou agendar.                                        |
| -[-] u <usuário [senha] > | Usuário a ser executado como. Inserir um \* para a senha produz um prompt para a senha. A senha não é exibida quando você a digita no prompt de senha. |
|           -y            |                                                      Responda sim a todas as perguntas sem avisar.                                                       |

## <a name="examples"></a>Exemplos  
O comando a seguir importa o arquivo XML c:\windows\ perf_log. XML do computador server_1 como um conjunto de coletores de dados chamado perf_log.  
```  
logman import perf_log -s server_1 -xml c:\windows\perf_log.xml  
```  
## <a name="additional-references"></a>Referências adicionais  
[logman](logman.md)  
