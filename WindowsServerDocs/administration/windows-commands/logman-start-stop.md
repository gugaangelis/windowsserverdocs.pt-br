---
title: inicialização de logman | deixar
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a40006a1-876e-474b-aaf1-f365c730deea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2bd81a33779aa58e7528d0173a7a4b49489de8f9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840619"
---
# <a name="logman-start--stop"></a>inicialização de logman | deixar

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
| -s <computer name> |            Execute o comando no computador remoto especificado.             |
|  -config <value>   |           Especifica o arquivo de configurações que contém as opções de comando.            |
|    [-n] <name>     |                          Nome do objeto de destino.                          |
|        -ETS        | Envie comandos para sessões de rastreamento de eventos diretamente sem salvar ou agendar. |
|        -como         |               Execute a operação solicitada de forma assíncrona.                |

## <a name="examples"></a><a name=BKMK_examples></a>Disso  
O comando a seguir inicia o coletor de dados perf_log no computador remoto server_1.  
```  
logman start perf_log -s server_1  
```  
## <a name="additional-references"></a>Referências adicionais  
[logman](logman.md)  
