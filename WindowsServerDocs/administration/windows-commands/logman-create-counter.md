---
title: Logman criar contador
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1e214c32-b704-43c1-b548-e1cf43b583c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3d9099fa4540a1d9c91a714ada8a1dbba13f051e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437814"
---
# <a name="logman-create-counter"></a>Logman criar contador

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crie um coletor de dados do contador.  

## <a name="syntax"></a>Sintaxe  
```  
logman create counter <[-n] <name>> [options]  
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
|                  -cf <filename>                  |                       Especifica o arquivo que lista os contadores de desempenho para coletar. O arquivo deve conter um nome de contador de desempenho por linha.                        |
|               -c <path [path [ ]]>               |                                                              Especifica os contadores de desempenho para coletar.                                                               |
|                   -sc <value>                    |                                      Especifica o número máximo de amostras a serem coletadas com um coletor de dados do contador de desempenho.                                      |

## <a name="remarks"></a>Comentários  
Onde [-] é listado, um - adicional nega a opção.  
## <a name="BKMK_examples"></a>Exemplos  
O comando a seguir cria um contador chamado perf_log usando o contador % de tempo de processador da categoria de contador ( total).  
```  
logman create counter perf_log -c "\Processor(_Total)\% Processor time"  
```  
O comando a seguir cria um contador chamado perf_log usando o contador % de tempo de processador da categoria de contador ( total), criando um arquivo de log com um tamanho máximo de 10 MB e coletando dados para 1 minuto e 0 segundos.  
```  
logman create counter perf_log -c "\Processor(_Total)\% Processor time" -max 10 -rf 01:00  
```  
#### <a name="additional-references"></a>Referências adicionais  
[logman](logman.md)  
