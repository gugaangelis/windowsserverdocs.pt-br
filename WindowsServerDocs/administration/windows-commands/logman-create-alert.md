---
title: criar alerta de logman
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 93e6fc2b-5bf5-413b-84b4-be8b9dd3a57d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2c56b8ee9ca9aa96cbff9af6f3726c62a21d7a17
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724432"
---
# <a name="logman-create-alert"></a>criar alerta de logman

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

criar um coletor de dados de alerta.  

## <a name="syntax"></a>Sintaxe  
```  
logman create alert <[-n] <name>> [options]  
```  
### <a name="parameters"></a>Parâmetros  

|                 Parâmetro                  |                                                                               Descrição                                                                               |
|--------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                     /?                     |                                                                    Exibe a ajuda contextual.                                                                     |
|             -s<computer name>             |                                                          Execute o comando no computador remoto especificado.                                                          |
|              -config <value>               |                                                         Especifica o arquivo de configurações que contém as opções de comando.                                                         |
|                [-n]<name>                 |                                                                       O nome do objeto de destino.                                                                        |
|          -[-] u <usuário [senha] >           | Especifica o usuário a ser executado como. Inserir um \* para a senha produz um prompt para a senha. A senha não é exibida quando você a digita no prompt de senha. |
| -m < [início] [parar] [[Iniciar] [parar] [...]] > |                                                Altere para início ou parada manual em vez de uma hora de início ou de término agendada.                                                 |
|             -RF < [[hh:] mm:] SS>             |                                                        Execute o coletor de dados para o período de tempo especificado.                                                         |
|     -b <M/d/AAAA h:mm: SS [AM&#124;PM] >      |                                                              Comece a coletar dados no horário especificado.                                                               |
|     -e <M/d/AAAA h:mm: SS [AM&#124;PM] >      |                                                               Terminar a coleta de dados na hora especificada.                                                                |
|             -si < [[hh:] mm:] SS>             |                                                 Especifica o intervalo de amostragem para coletores de dados de contador de desempenho.                                                  |
|           -o caminho de <&#124;DSN! log>           |                                              Especifica o arquivo de log de saída ou o DSN e o nome do conjunto de logs em um banco de dados SQL.                                               |
|                   -[-] r                    |                                                  Repita o coletor de dados diariamente nas horas de início e término especificadas.                                                  |
|                   -[-] um                    |                                                                     anexar a um arquivo de log existente.                                                                     |
|                   -[-] Omo                   |                                                                     Substituir um arquivo de log existente.                                                                     |
|        -[-] v <nnnnnn&#124;mmddhhmm>        |                                                   Anexe informações de controle de versão do arquivo ao final do nome do arquivo de log.                                                   |
|               -[-] RC<task>                |                                                         Execute o comando especificado cada vez que o log for fechado.                                                          |
|              -[-] máx. <value>               |                                                 Tamanho máximo do arquivo de log em MB ou número máximo de registros para logs SQL.                                                  |
|           -[-] CNF < [[hh:] mm:] SS>           |     Quando o tempo for especificado, crie um novo arquivo quando o tempo especificado tiver decorrido. Quando a hora não for especificada, crie um novo arquivo quando o tamanho máximo for excedido.     |
|                     -y                     |                                                             Responda sim a todas as perguntas sem avisar.                                                              |
|               -CF<filename>               |                       Especifica o arquivo que lista os contadores de desempenho a serem coletados. O arquivo deve conter um nome de contador de desempenho por linha.                        |
|                   -[-] El                   |                                                                Habilita ou desabilita o relatório do log de eventos.                                                                 |
|     -ésimo <limite [limite [...]] >      |                                                        Especifique os contadores e seus valores limites para um alerta.                                                        |
|              -[-]rdcs<name>               |                                                     Especifica o conjunto de coletores de dados a ser iniciado quando um alerta é disparado.                                                      |
|               -[-] TN<task>                |                                                             Especifica a tarefa a ser executada quando um alerta é disparado.                                                              |
|            -[-] Tino<argument>             |                                               Especifica os argumentos da tarefa a serem usados com a tarefa especificada usando-TN.                                                |

## <a name="remarks"></a>Comentários  
Onde [-] está listado, um extra-nega a opção.  
## <a name="examples"></a>Exemplos  
O comando a seguir cria um alerta chamado new_alert que é acionado quando o contador de desempenho% tempo do processador no grupo de contadores processador (_Total) excede o valor do contador de 50.  
```  
logman create alert new_alert -th \Processor(_Total)\% Processor time>50  
```  
> [!NOTE]
> O valor de limite definido é baseado no valor coletado pelo contador, portanto, neste exemplo, o valor de 50 equivale a 50% de tempo do processador.  
> ## <a name="additional-references"></a>Referências adicionais  
> [logman](logman.md)  
