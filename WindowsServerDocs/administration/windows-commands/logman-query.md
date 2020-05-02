---
title: logman query
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1116a0f0-5415-4369-a045-12f79f8f66de
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 05448a4f129a59145813dd0da7199d4adf845c5c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724351"
---
# <a name="logman-query"></a>logman query

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

consultar as propriedades do coletor de dados ou do conjunto de coletores de dados.  

## <a name="syntax"></a>Sintaxe  
```  
logman query [providers|Data Collector Set name] [options]  
```  
### <a name="parameters"></a>Parâmetros  

|     Parâmetro      |                                 Descrição                                  |
|--------------------|------------------------------------------------------------------------------|
|         /?         |                       Exibe a ajuda contextual.                       |
| -s<computer name> |            Execute o comando no computador remoto especificado.             |
|  -config <value>   |           Especifica o arquivo de configurações que contém as opções de comando.            |
|    [-n]<name>     |                          O nome do objeto de destino.                          |
|        -ETS        | Envie comandos para sessões de rastreamento de eventos diretamente sem salvar ou agendar. |

## <a name="examples"></a>Exemplos  
O comando a seguir lista todos os conjuntos de coletores de dados configurados no sistema de destino.  
```  
logman query  
```  
O comando a seguir lista os coletores de dados contidos no conjunto de coletores de dados denominado perf_log.  
```  
logman query perf_log  
```  
O comando a seguir lista todos os provedores disponíveis de coletores de dados no sistema de destino.  
```  
logman query providers  
```  
## <a name="additional-references"></a>Referências adicionais  
[logman](logman.md)  
