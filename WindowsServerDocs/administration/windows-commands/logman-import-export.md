---
title: importação de logman | Export
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c258daba-fb93-47c0-a53b-2fe83ed2c743
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 81147f9e2e2da69c8e59969f3c176264a7fa353a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840669"
---
# <a name="logman-import--export"></a>importação de logman | Export

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
|   -s <computer name>    |                                                   Execute o comando no computador remoto especificado.                                                   |
|     -config <value>     |                                                  Especifica o arquivo de configurações que contém as opções de comando.                                                  |
|       [-n] <name>       |                                                                Nome do objeto de destino.                                                                 |
|       -XML <name>       |                                                         Nome do arquivo XML a ser importado ou exportado.                                                         |
|          -ETS           |                                       Envie comandos para sessões de rastreamento de eventos diretamente sem salvar ou agendar.                                        |
| -[-] u < usuário [senha] > | Usuário a ser executado como. Inserir um \* para a senha produz uma solicitação para a senha. A senha não é exibida quando você a digita no prompt de senha. |
|           -y            |                                                      Responda sim a todas as perguntas sem avisar.                                                       |

## <a name="examples"></a><a name=BKMK_examples></a>Disso  
O comando a seguir importa o arquivo XML c:\windows\ perf_log. XML do computador server_1 como um conjunto de coletores de dados chamado perf_log.  
```  
logman import perf_log -s server_1 -xml c:\windows\perf_log.xml  
```  
## <a name="additional-references"></a>Referências adicionais  
[logman](logman.md)  
