---
title: rastreamento de atualização de logman
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b7111f7f-4162-4d1a-8e53-d766db0ede1f britw
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 30fe475fb6a8442558493dcd5a91ecb8fcee2de3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826427"
---
# <a name="logman-update-trace"></a>rastreamento de atualização de logman

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Atualize as propriedades de um coletor de dados de rastreamento de eventos existente.  
  
## <a name="syntax"></a>Sintaxe  
```  
logman update trace <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|/?|Exibe contextual a Ajuda.|  
|-s <computer name>|Execute o comando no computador remoto especificado.|  
|-config <value>|Especifica o arquivo de configurações que contém opções de comando.|  
|-ets|Envie comandos para sessões de rastreamento de eventos diretamente sem salvar ou agendamento.|  
|[-n] <name>|Nome do objeto de destino.|  
|-f < bin&#124;bincirc&#124;csv&#124;tsv&#124;sql >|Especifica o formato de log para o coletor de dados.|  
|-u [-] < usuário [senha] >|Especifica o usuário executar como. Inserir um * para a senha produz um prompt para a senha. A senha não é exibida quando ela for digitada no prompt de senha.|  
|-m <[start] [stop] [[start] [stop] [...]]>|Alterar para inicialização manual ou parar em vez de um horário agendado begin ou end.|  
|-rf <[[hh:]mm:]ss>|Execute o coletor de dados para o período de tempo especificado.|  
|-b < m/aaaa h:mm: ss [AM&#124;PM] >|Começar a coleta de dados no momento especificado.|  
|-e < dd/aaaa h:mm: ss [AM&#124;PM] >|Encerrar a coleta de dados no momento especificado.|  
|-s < caminho&#124;dsn! log >|Especifica que o arquivo de log de saída ou DSN e log de nome de conjunto em um banco de dados SQL.|  
|-[-]r|Repita o coletor de dados diariamente às específicos de início e término.|  
|-[-]a|anexa a um arquivo de log existente.|  
|-[-]ow|Substitua um arquivo de log existente.|  
|-[-]v <nnnnnn&#124;mmddhhmm>|Anexe informações de controle de versão do arquivo até o final do nome do arquivo de log.|  
|-[-]rc <task>|Execute o comando especificado sempre que o log é fechado.|  
|-[-]max <value>|Tamanho do arquivo de log máximo em MB ou o número máximo de registros de logs do SQL.|  
|-[-]cnf <[[hh:]mm:]ss>|Quando a hora for especificada, crie um novo arquivo quando o tempo especificado tiver decorrido. Quando o tempo não for especificado, crie um novo arquivo quando o tamanho máximo for excedido.|  
|-y|Responda Sim para todas as perguntas sem avisar.|  
|-ct < perf&#124;sistema&#124;ciclo >|Especifica o tipo de relógio de sessão de rastreamento de eventos.|  
|-ln <logger_name>|Especifica o nome do agente para sessões de rastreamento de eventos.|  
|-ft <[[hh:]mm:]ss>|Especifica o temporizador de liberação de sessão de rastreamento de eventos.|  
|-p [-] < provedor [flags [nível]] >|Especifica um único provedor de rastreamento de eventos para habilitar.|  
|-pf <filename>|Especifica um arquivo de listagem de vários provedores de rastreamento de eventos para habilitar. O arquivo deve ser um arquivo de texto que contém um provedor por linha.|  
|-[-]rt|Execute a sessão de rastreamento de eventos no modo em tempo real.|  
|-[-]ul|Execute a sessão de rastreamento de eventos no modo de usuário.|  
|-bs <value>|Especifica o tamanho do buffer de sessão de rastreamento de evento em kb.|  
|-nb <min max>|Especifica o número de buffers de sessão de rastreamento de eventos.|  
|-modo < globalsequence&#124;localsequence&#124;pagedmemory >|Especifica o modo de agente de sessão de rastreamento de eventos.<br /><br />**Globalsequence** Especifica que o rastreador de eventos adicionar um número de sequência para cada evento recebido, independentemente de qual rastreamento de sessão recebeu o evento.<br /><br />**Localsequence** Especifica que o rastreador de eventos adicionar números de sequência de eventos recebidos em uma sessão de rastreamento específico. Quando o **localsequence** opção for usada, os números de sequência duplicada podem existir em todas as sessões, mas serão ser exclusivos dentro de cada sessão de rastreamento.<br /><br />**Pagedmemory** Especifica que o rastreador de eventos usa a memória paginável em vez do pool de memória não paginada padrão para suas alocações de buffer interno.|  
## <a name="remarks"></a>Comentários  
Onde [-] é listado, um - adicional nega a opção.  
## <a name="BKMK_examples"></a>Exemplos  
O comando a seguir atualiza o existente perf_log de coletor de dados, alterando o tamanho máximo do log para 10 MB, atualizando o formato de arquivo de log para CSV e acrescentar o controle de versão do arquivo no mmddhhmm formato.  
```  
logman update perf_log -max 10 -f csv -v mmddhhmm  
```  
#### <a name="additional-references"></a>Referências adicionais  
[logman](logman.md)  
[Logman criar rastreamento](logman-create-trace.md)  
