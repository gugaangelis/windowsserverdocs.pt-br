---
title: criar contador de logman
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1e214c32-b704-43c1-b548-e1cf43b583c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: feff7ad1597232ad441e2ad23ee73638b4ad68ea
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724394"
---
# <a name="logman-create-counter"></a>criar contador de logman

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

criar um coletor de dados de contador.  

## <a name="syntax"></a>Sintaxe  
```  
logman create counter <[-n] <name>> [options]  
```  
### <a name="parameters"></a>Parâmetros  

|                    Parâmetro                     |                                                                               Descrição                                                                               |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                        /?                        |                                                                    Exibe a ajuda contextual.                                                                     |
|                -s<computer name>                |                                                          Execute o comando no computador remoto especificado.                                                          |
|                 -config <value>                  |                                                         Especifica o arquivo de configurações que contém as opções de comando.                                                         |
|                   [-n]<name>                    |                                                                       O nome do objeto de destino.                                                                        |
| -f <bin&#124;bincirc&#124;CSV&#124;TSV&#124;SQL> |                                                            Especifica o formato de log para o coletor de dados.                                                             |
|             -[-] u <usuário [senha] >              | Especifica o usuário a ser executado como. Inserir um \* para a senha produz um prompt para a senha. A senha não é exibida quando você a digita no prompt de senha. |
|    -m < [início] [parar] [[Iniciar] [parar] [...]] >    |                                                Altere para início ou parada manual em vez de uma hora de início ou de término agendada.                                                 |
|                -RF < [[hh:] mm:] SS>                |                                                        Execute o coletor de dados para o período de tempo especificado.                                                         |
|        -b <M/d/AAAA h:mm: SS [AM&#124;PM] >         |                                                              Comece a coletar dados no horário especificado.                                                               |
|        -e <M/d/AAAA h:mm: SS [AM&#124;PM] >         |                                                               Terminar a coleta de dados na hora especificada.                                                                |
|                -si < [[hh:] mm:] SS>                |                                                 Especifica o intervalo de amostragem para coletores de dados de contador de desempenho.                                                  |
|              -o caminho de <&#124;DSN! log>              |                                              Especifica o arquivo de log de saída ou o DSN e o nome do conjunto de logs em um banco de dados SQL.                                               |
|                      -[-] r                       |                                                  Repita o coletor de dados diariamente nas horas de início e término especificadas.                                                  |
|                      -[-] um                       |                                                                     anexar a um arquivo de log existente.                                                                     |
|                      -[-] Omo                      |                                                                     Substituir um arquivo de log existente.                                                                     |
|           -[-] v <nnnnnn&#124;mmddhhmm>           |                                                   Anexe informações de controle de versão do arquivo ao final do nome do arquivo de log.                                                   |
|                  -[-] RC<task>                   |                                                         Execute o comando especificado cada vez que o log for fechado.                                                          |
|                 -[-] máx. <value>                  |                                                 Tamanho máximo do arquivo de log em MB ou número máximo de registros para logs SQL.                                                  |
|              -[-] CNF < [[hh:] mm:] SS>              |     Quando o tempo for especificado, crie um novo arquivo quando o tempo especificado tiver decorrido. Quando a hora não for especificada, crie um novo arquivo quando o tamanho máximo for excedido.     |
|                        -y                        |                                                             Responda sim a todas as perguntas sem avisar.                                                              |
|                  -CF<filename>                  |                       Especifica o arquivo que lista os contadores de desempenho a serem coletados. O arquivo deve conter um nome de contador de desempenho por linha.                        |
|               -c <caminho [caminho []] >               |                                                              Especifica os contadores de desempenho a serem coletados.                                                               |
|                   -SC <value>                    |                                      Especifica o número máximo de amostras a serem coletadas com um coletor de dados de contador de desempenho.                                      |

## <a name="remarks"></a>Comentários  
Onde [-] está listado, um extra-nega a opção.  
## <a name="examples"></a>Exemplos  
O comando a seguir cria um contador chamado perf_log usando o contador% tempo do processador da categoria de contador processador (_Total).  
```  
logman create counter perf_log -c \Processor(_Total)\% Processor time  
```  
O comando a seguir cria um contador chamado perf_log usando o contador% tempo do processador da categoria de contador processador (_Total), criando um arquivo de log com um tamanho máximo de 10 MB e coletando dados por 1 minuto e 0 segundo.  
```  
logman create counter perf_log -c \Processor(_Total)\% Processor time -max 10 -rf 01:00  
```  
## <a name="additional-references"></a>Referências adicionais  
[logman](logman.md)  
