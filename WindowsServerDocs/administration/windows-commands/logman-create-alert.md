---
title: Logman criar alerta
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 93e6fc2b-5bf5-413b-84b4-be8b9dd3a57d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 37a13ab5623a295f96bde2f734bcb17e1eca2be9
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437838"
---
# <a name="logman-create-alert"></a>Logman criar alerta

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crie um coletor de dados de alerta.  

## <a name="syntax"></a>Sintaxe  
```  
logman create alert <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Parâmetros  

|                 Parâmetro                  |                                                                               Descrição                                                                               |
|--------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                     /?                     |                                                                    Exibe contextual a Ajuda.                                                                     |
|             -s <computer name>             |                                                          Execute o comando no computador remoto especificado.                                                          |
|              -config <value>               |                                                         Especifica o arquivo de configurações que contém opções de comando.                                                         |
|                [-n] <name>                 |                                                                       Nome do objeto de destino.                                                                        |
|          -u [-] < usuário [senha] >           | Especifica o usuário executar como. Inserindo um \* User um prompt para a senha. A senha não é exibida quando ela for digitada no prompt de senha. |
| -m <[start] [stop] [[start] [stop] [...]]> |                                                Alterar para inicialização manual ou parar em vez de um horário agendado begin ou end.                                                 |
|             -rf <[[hh:]mm:]ss>             |                                                        Execute o coletor de dados para o período de tempo especificado.                                                         |
|     -b < m/aaaa h:mm: ss [AM&#124;PM] >      |                                                              Começar a coleta de dados no momento especificado.                                                               |
|     -e < dd/aaaa h:mm: ss [AM&#124;PM] >      |                                                               Encerrar a coleta de dados no momento especificado.                                                                |
|             -si <[[hh:]mm:]ss>             |                                                 Especifica o intervalo de amostragem para Coletores de dados do contador de desempenho.                                                  |
|           -s < caminho&#124;dsn! log >           |                                              Especifica que o arquivo de log de saída ou DSN e log de nome de conjunto em um banco de dados SQL.                                               |
|                   -[-]r                    |                                                  Repita o coletor de dados diariamente às específicos de início e término.                                                  |
|                   -[-]a                    |                                                                     anexa a um arquivo de log existente.                                                                     |
|                   -[-]ow                   |                                                                     Substitua um arquivo de log existente.                                                                     |
|        -[-]v <nnnnnn&#124;mmddhhmm>        |                                                   Anexe informações de controle de versão do arquivo até o final do nome do arquivo de log.                                                   |
|               -[-]rc <task>                |                                                         Execute o comando especificado sempre que o log é fechado.                                                          |
|              -[-]max <value>               |                                                 Tamanho do arquivo de log máximo em MB ou o número máximo de registros de logs do SQL.                                                  |
|           -[-]cnf <[[hh:]mm:]ss>           |     Quando a hora for especificada, crie um novo arquivo quando o tempo especificado tiver decorrido. Quando o tempo não for especificado, crie um novo arquivo quando o tamanho máximo for excedido.     |
|                     -y                     |                                                             Responda Sim para todas as perguntas sem avisar.                                                              |
|               -cf <filename>               |                       Especifica o arquivo que lista os contadores de desempenho para coletar. O arquivo deve conter um nome de contador de desempenho por linha.                        |
|                   -[-]el                   |                                                                Habilita ou desabilita o relatório de Log de eventos.                                                                 |
|     -ésimo < limite [limite [...]] >      |                                                        Especifica contadores e seus valores de limite para um alerta.                                                        |
|              -[-]rdcs <name>               |                                                     Especifica o conjunto de Coletores de dados para iniciar quando um alerta é disparado.                                                      |
|               -[-]tn <task>                |                                                             Especifica a tarefa seja executada quando um alerta for acionado.                                                              |
|            -[-]targ <argument>             |                                               Especifica os argumentos de tarefa a ser usado com a tarefa especificada usando -tn.                                                |

## <a name="remarks"></a>Comentários  
Onde [-] é listado, um - adicional nega a opção.  
## <a name="BKMK_examples"></a>Exemplos  
O comando a seguir cria um alerta chamado new_alert que é acionado quando o desempenho contador % tempo do processador no grupo de contador ( total) excede o valor do contador de 50.  
```  
logman create alert new_alert -th "\Processor(_Total)\% Processor time>50"  
```  
> [!NOTE]
> O valor do limite definido baseia-se no valor coletado pelo contador, portanto, neste exemplo, o valor de 50 é igual a 50% tempo do processador.  
> #### <a name="additional-references"></a>Referências adicionais  
> [logman](logman.md)  
