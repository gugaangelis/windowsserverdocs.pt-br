---
title: logman query
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1116a0f0-5415-4369-a045-12f79f8f66de
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 79e23e15d85e7ab50d651baf7a556cc76c64fc26
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876657"
---
# <a name="logman-query"></a>logman query

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

coletor de dados de consulta ou o coletor de dados definir propriedades.  
  
## <a name="syntax"></a>Sintaxe  
```  
logman query [providers|"Data Collector Set name"] [options]  
```  
## <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|/?|Exibe contextual a Ajuda.|  
|-s <computer name>|Execute o comando no computador remoto especificado.|  
|-config <value>|Especifica o arquivo de configurações que contém opções de comando.|  
|[-n] <name>|Nome do objeto de destino.|  
|-ets|Envie comandos para sessões de rastreamento de eventos diretamente sem salvar ou agendamento.|  
## <a name="BKMK_examples"></a>Exemplos  
O comando a seguir lista todos os conjuntos de Coletores de dados configurado no sistema de destino.  
```  
logman query  
```  
O comando a seguir lista os coletores de dados contidos no conjunto de Coletores de dados chamado perf_log.  
```  
logman query "perf_log"  
```  
O comando a seguir lista todos os provedores disponíveis de Coletores de dados no sistema de destino.  
```  
logman query providers  
```  
#### <a name="additional-references"></a>Referências adicionais  
[logman](logman.md)  
