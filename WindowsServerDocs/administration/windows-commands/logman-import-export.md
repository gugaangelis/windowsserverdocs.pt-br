---
title: Logman import | Exportar
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 67621b109c379cc2758b4036460b6bb82df3e3d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848297"
---
# <a name="logman-import--export"></a>Logman import | Exportar

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Importar um conjunto de Coletores de dados de um arquivo XML ou exportar um conjunto de Coletores de dados para um arquivo XML.  
  
## <a name="syntax"></a>Sintaxe  
```  
logman import <[-n] <name>> <-xml <name>> [options]  
logman export <[-n] <name>> <-xml <name>> [options]  
```  
## <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|-?|Exibe contextual a Ajuda.|  
|-s <computer name>|Execute o comando no computador remoto especificado.|  
|-config <value>|Especifica o arquivo de configurações que contém opções de comando.|  
|[-n] <name>|Nome do objeto de destino.|  
|-xml <name>|Nome do arquivo XML para importar ou exportar.|  
|-ets|Envie comandos para sessões de rastreamento de eventos diretamente sem salvar ou agendamento.|  
|-u [-] < usuário [senha] >|Usuário executar como. Inserir um * para a senha produz um prompt para a senha. A senha não é exibida quando ela for digitada no prompt de senha.|  
|-y|Responda Sim para todas as perguntas sem avisar.|  
## <a name="BKMK_examples"></a>Exemplos  
O comando a seguir importa o XML arquivo c:\windows\perf_log.xml de server_1 o computador como um coletor de dados chamado perf_log do conjunto.  
```  
logman import perf_log -s server_1 -xml "c:\windows\perf_log.xml"  
```  
#### <a name="additional-references"></a>Referências adicionais  
[logman](logman.md)  
