---
title: Logman start | parar
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a40006a1-876e-474b-aaf1-f365c730deea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0c6027c4c9a99e45bb1c2e95cdfd4a7687a5c43b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437685"
---
# <a name="logman-start--stop"></a>Logman start | parar

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Iniciar um coletor de dados e definir a hora de início para manual ou parar um coletor de dados definido e defina a hora de término como manual.  

## <a name="syntax"></a>Sintaxe  
```  
logman start <[-n] <name>> [options]  
logman stop <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Parâmetros  

|     Parâmetro      |                                 Descrição                                  |
|--------------------|------------------------------------------------------------------------------|
|         -?         |                       Exibe contextual a Ajuda.                       |
| -s <computer name> |            Execute o comando no computador remoto especificado.             |
|  -config <value>   |           Especifica o arquivo de configurações que contém opções de comando.            |
|    [-n] <name>     |                          Nome do objeto de destino.                          |
|        -ets        | Envie comandos para sessões de rastreamento de eventos diretamente sem salvar ou agendamento. |
|        -as         |               Execute a operação solicitada de forma assíncrona.                |

## <a name="BKMK_examples"></a>Exemplos  
O comando a seguir inicia o perf_log do coletor de dados no server_1 computador remoto.  
```  
logman start perf_log -s server_1  
```  
#### <a name="additional-references"></a>Referências adicionais  
[logman](logman.md)  
