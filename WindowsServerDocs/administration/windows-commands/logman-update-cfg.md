---
title: Logman update cfg
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9da4e8b4-3be5-42d3-b0b4-c429630c35c4 britw
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5fce710afa5e05bbc68d5e6c4b796caa3fca3d2f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843377"
---
# <a name="logman-update-cfg"></a>Logman update cfg

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Atualize as propriedades de um coletor de dados de configuração existente.  
  
## <a name="syntax"></a>Sintaxe  
```  
logman update cfg <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|/?|Exibe contextual a Ajuda.|  
|-s <computer name>|Execute o comando no computador remoto especificado.|  
|-config <value>|Especifica o arquivo de configurações que contém opções de comando.|  
|[-n] <name>|Nome do objeto de destino.|  
|-f < bin&#124;bincirc&#124;csv&#124;tsv&#124;sql >|Especifica o formato de log para o coletor de dados.|  
|-u [-] < usuário [senha] >|Especifica o usuário executar como. Inserir um * para a senha produz um prompt para a senha. A senha não é exibida quando ela for digitada no prompt de senha.|  
|-m <[start] [stop] [[start] [stop] [...]]>|Alterar para inicialização manual ou parar em vez de um horário agendado begin ou end.|  
|-rf <[[hh:]mm:]ss>|Execute o coletor de dados para o período de tempo especificado.|  
|-b < m/aaaa h:mm: ss [AM&#124;PM] >|Começar a coleta de dados no momento especificado.|  
|-e < dd/aaaa h:mm: ss [AM&#124;PM] >|Encerrar a coleta de dados no momento especificado.|  
|-si <[[hh:]mm:]ss>|Especifica o intervalo de amostragem para Coletores de dados do contador de desempenho.|  
|-s < caminho&#124;dsn! log >|Especifica que o arquivo de log de saída ou DSN e log de nome de conjunto em um banco de dados SQL.|  
|-[-]r|Repita o coletor de dados diariamente às específicos de início e término.|  
|-[-]a|anexa a um arquivo de log existente.|  
|-[-]ow|Substitua um arquivo de log existente.|  
|-[-]v <nnnnnn&#124;mmddhhmm>|Anexe informações de controle de versão do arquivo até o final do nome do arquivo de log.|  
|-[-]rc <task>|Execute o comando especificado sempre que o log é fechado.|  
|-[-]max <value>|Tamanho do arquivo de log máximo em MB ou o número máximo de registros de logs do SQL.|  
|-[-]cnf <[[hh:]mm:]ss>|Quando a hora for especificada, crie um novo arquivo quando o tempo especificado tiver decorrido. Quando o tempo não for especificado, crie um novo arquivo quando o tamanho máximo for excedido.|  
|-y|Responda Sim para todas as perguntas sem avisar.|  
|-[-]ni|Habilitar (-ni) ou desabilitar (-ni) consulta de interface de rede.|  
|-reg <path [path [...]]>|Especifica o valor (es) de registro para coletar.|  
|-mgt <query [query [...]]>|Especifica o objeto (s) WMI para coletar usando a linguagem de consulta SQL.|  
|-ftc <path [path [...]]>|Especifica o caminho completo para o arquivo (s) para coletar.|  
## <a name="remarks"></a>Comentários  
Onde [-] é listado, um - adicional nega a opção.  
## <a name="BKMK_examples"></a>Exemplos  
O comando a seguir atualiza o cfg_log existente de coletor de dados de configuração para coletar a chave do Registro HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\Currentverion\\.  
```  
logman update cfg cfg_log -reg "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\Currentverion\"  
```  
#### <a name="additional-references"></a>Referências adicionais  
[logman](logman.md)  
[Logman criar cfg](logman-create-cfg.md)  
