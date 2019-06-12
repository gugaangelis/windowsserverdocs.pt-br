---
title: Logman atualizar api
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6f322e52-0f9f-42b1-bd64-8b8f8fe086fc britw
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 285e4b527cf02061380ab2d9b5525e5b297a43cc
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437659"
---
# <a name="logman-update-api"></a>Logman atualizar api

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Atualize as propriedades de um coletor de dados de rastreamento de API existente.  

## <a name="syntax"></a>Sintaxe  
```  
logman update api <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Parâmetros  

|                    Parâmetro                     |                                                                               Descrição                                                                               |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                        /?                        |                                                                    Exibe contextual a Ajuda.                                                                     |
|                -s <computer name>                |                                                          Execute o comando no computador remoto especificado.                                                          |
|                 -config <value>                  |                                                         Especifica o arquivo de configurações que contém opções de comando.                                                         |
|                   [-n] <name>                    |                                                                       Nome do objeto de destino.                                                                        |
| -f < bin&#124;bincirc&#124;csv&#124;tsv&#124;sql > |                                                            Especifica o formato de log para o coletor de dados.                                                             |
|             -u [-] < usuário [senha] >              | Especifica o usuário executar como. Inserindo um \* User um prompt para a senha. A senha não é exibida quando ela for digitada no prompt de senha. |
|    -m <[start] [stop] [[start] [stop] [...]]>    |                                                Alterar para inicialização manual ou parar em vez de um horário agendado begin ou end.                                                 |
|                -rf <[[hh:]mm:]ss>                |                                                        Execute o coletor de dados para o período de tempo especificado.                                                         |
|        -b < m/aaaa h:mm: ss [AM&#124;PM] >         |                                                              Começar a coleta de dados no momento especificado.                                                               |
|        -e < dd/aaaa h:mm: ss [AM&#124;PM] >         |                                                               Encerrar a coleta de dados no momento especificado.                                                                |
|                -si <[[hh:]mm:]ss>                |                                                 Especifica o intervalo de amostragem para Coletores de dados do contador de desempenho.                                                  |
|              -s < caminho&#124;dsn! log >              |                                              Especifica que o arquivo de log de saída ou DSN e log de nome de conjunto em um banco de dados SQL.                                               |
|                      -[-]r                       |                                                  Repita o coletor de dados diariamente às específicos de início e término.                                                  |
|                      -[-]a                       |                                                                     anexa a um arquivo de log existente.                                                                     |
|                      -[-]ow                      |                                                                     Substitua um arquivo de log existente.                                                                     |
|           -[-]v <nnnnnn&#124;mmddhhmm>           |                                                   Anexe informações de controle de versão do arquivo até o final do nome do arquivo de log.                                                   |
|                  -[-]rc <task>                   |                                                         Execute o comando especificado sempre que o log é fechado.                                                          |
|                 -[-]max <value>                  |                                                 Tamanho do arquivo de log máximo em MB ou o número máximo de registros de logs do SQL.                                                  |
|              -[-]cnf <[[hh:]mm:]ss>              |     Quando a hora for especificada, crie um novo arquivo quando o tempo especificado tiver decorrido. Quando o tempo não for especificado, crie um novo arquivo quando o tamanho máximo for excedido.     |
|                        -y                        |                                                             Responda Sim para todas as perguntas sem avisar.                                                              |
|            -mods <path [path [...]]>             |                                                          Especifica a lista de módulos para fazer chamadas da API do.                                                           |
|     -inapis <module!api [module!api [...]]>      |                                                         Especifica a lista de chamadas de API para incluir no registro em log.                                                          |
|     -exapis <module!api [module!api [...]]>      |                                                        Especifica a lista de chamadas de API para excluir do registro em log.                                                         |
|                     -[-]ano                      |                                                     Log (-ano), nomes de API ou não registrar em log somente (-ano) nomes de API.                                                     |
|                  -[-]recursive                   |                                          Log (-recursiva) ou não registrar em log (-recursivo) APIs recursivamente além da primeira camada.                                           |
|                   -exe <value>                   |                                                        Especifica o caminho completo para um executável para rastreamento de API.                                                        |

## <a name="remarks"></a>Comentários  
Onde [-] é listado, um - adicional nega a opção.  
## <a name="BKMK_examples"></a>Exemplos  
O seguinte contador de rastreamento de atualizações a API existente de comando chamado trace_notepad para o arquivo executável c:\Windows\Notepad.exe. excluindo a chamada à API TlsGetValue produzido pelo kernel32.dll módulo.  
```  
logman create api trace_notepad -exe c:\windows\notepad.exe -exapis kernel32.dll!TlsGetValue  
```  
#### <a name="additional-references"></a>Referências adicionais  
[logman](logman.md)  
[Logman criar api](logman-create-api.md)  
