---
title: inicialização de logman | deixar
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 395d325b31ee596e1394e7ed796a444f159d15fc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374412"
---
# <a name="logman-start--stop"></a>inicialização de logman | deixar

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Inicie um coletor de dados e defina a hora de início como manual ou pare um conjunto de coletores de dados e defina a hora de término como manual.  

## <a name="syntax"></a>Sintaxe  
```  
logman start <[-n] <name>> [options]  
logman stop <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Parâmetros  

|     Parâmetro      |                                 Descrição                                  |
|--------------------|------------------------------------------------------------------------------|
|         -?         |                       Exibe a ajuda contextual.                       |
| -s <computer name> |            Execute o comando no computador remoto especificado.             |
|  -config <value>   |           Especifica o arquivo de configurações que contém as opções de comando.            |
|    [-n] <name>     |                          Nome do objeto de destino.                          |
|        -ETS        | Envie comandos para sessões de rastreamento de eventos diretamente sem salvar ou agendar. |
|        -como         |               Execute a operação solicitada de forma assíncrona.                |

## <a name="BKMK_examples"></a>Disso  
O comando a seguir inicia o Data Collector perf_log no computador remoto server_1.  
```  
logman start perf_log -s server_1  
```  
#### <a name="additional-references"></a>Referências adicionais  
[logman](logman.md)  
