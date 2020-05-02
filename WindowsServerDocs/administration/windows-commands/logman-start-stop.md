---
title: inicialização de logman | deixar
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a40006a1-876e-474b-aaf1-f365c730deea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 68f570d99d4b3eaa818c9fbdcce76c42d1cb12d4
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724341"
---
# <a name="logman-start--stop"></a>inicialização de logman | deixar

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Inicie um coletor de dados e defina a hora de início como manual ou pare um conjunto de coletores de dados e defina a hora de término como manual.  

## <a name="syntax"></a>Sintaxe  
```  
logman start <[-n] <name>> [options]  
logman stop <[-n] <name>> [options]  
```  
### <a name="parameters"></a>Parâmetros  

|     Parâmetro      |                                 Descrição                                  |
|--------------------|------------------------------------------------------------------------------|
|         -?         |                       Exibe a ajuda contextual.                       |
| -s<computer name> |            Execute o comando no computador remoto especificado.             |
|  -config <value>   |           Especifica o arquivo de configurações que contém as opções de comando.            |
|    [-n]<name>     |                          O nome do objeto de destino.                          |
|        -ETS        | Envie comandos para sessões de rastreamento de eventos diretamente sem salvar ou agendar. |
|        -como         |               Execute a operação solicitada de forma assíncrona.                |

## <a name="examples"></a>Exemplos  
O comando a seguir inicia o coletor de dados perf_log no computador remoto server_1.  
```  
logman start perf_log -s server_1  
```  
## <a name="additional-references"></a>Referências adicionais  
[logman](logman.md)  
