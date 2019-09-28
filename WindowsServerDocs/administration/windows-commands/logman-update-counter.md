---
title: contador de atualização do logman
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 607df6d5-876c-428d-a0b3-f59cb244e2ce britw
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8c0696b6077a919d93106cb39329c986e91883fa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374348"
---
# <a name="logman-update-counter"></a>contador de atualização do logman

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Atualize as propriedades de um coletor de dados do contador existente.  

## <a name="syntax"></a>Sintaxe  
```  
logman update counter <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Parâmetros  

|                    Parâmetro                     |                                                                               Descrição                                                                               |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                        /?                        |                                                                    Exibe a ajuda contextual.                                                                     |
|                -s <computer name>                |                                                          Execute o comando no computador remoto especificado.                                                          |
|                 -config <value>                  |                                                         Especifica o arquivo de configurações que contém as opções de comando.                                                         |
|                   [-n] <name>                    |                                                                       Nome do objeto de destino.                                                                        |
| -f < bin&#124;bincirc&#124;CSV&#124;TSV&#124;SQL > |                                                            Especifica o formato de log para o coletor de dados.                                                             |
|             -[-] u < usuário [senha] >              | Especifica o usuário a ser executado como. Inserir um \* para a senha produz uma solicitação para a senha. A senha não é exibida quando você a digita no prompt de senha. |
|    -m < [início] [parar] [[Iniciar] [parar] [...]] >    |                                                Altere para início ou parada manual em vez de uma hora de início ou de término agendada.                                                 |
|                -RF < [[hh:] mm:] SS >                |                                                        Execute o coletor de dados para o período de tempo especificado.                                                         |
|        -b < M/d/AAAA h:mm: SS [AM&#124;PM] >         |                                                              Comece a coletar dados no horário especificado.                                                               |
|        -e < M/d/AAAA h:mm: SS [AM&#124;PM] >         |                                                               Terminar a coleta de dados na hora especificada.                                                                |
|                -si < [[hh:] mm:] SS >                |                                                 Especifica o intervalo de amostragem para coletores de dados de contador de desempenho.                                                  |
|              -o < DSN&#124;de caminho! log >              |                                              Especifica o arquivo de log de saída ou o DSN e o nome do conjunto de logs em um banco de dados SQL.                                               |
|                      -[-] r                       |                                                  Repita o coletor de dados diariamente nas horas de início e término especificadas.                                                  |
|                      -[-] um                       |                                                                     anexar a um arquivo de log existente.                                                                     |
|                      -[-] Omo                      |                                                                     Substituir um arquivo de log existente.                                                                     |
|           -[-] v < nnnnnn&#124;mmddhhmm >           |                                                   Anexe informações de controle de versão do arquivo ao final do nome do arquivo de log.                                                   |
|                  -[-] RC <task>                   |                                                         Execute o comando especificado cada vez que o log for fechado.                                                          |
|                 -[-] máx. <value>                  |                                                 Tamanho máximo do arquivo de log em MB ou número máximo de registros para logs SQL.                                                  |
|              -[-] CNF < [[hh:] mm:] SS >              |     Quando o tempo for especificado, crie um novo arquivo quando o tempo especificado tiver decorrido. Quando a hora não for especificada, crie um novo arquivo quando o tamanho máximo for excedido.     |
|                        -y                        |                                                             Responda sim a todas as perguntas sem avisar.                                                              |
|                  -CF <filename>                  |                       Especifica o arquivo que lista os contadores de desempenho a serem coletados. O arquivo deve conter um nome de contador de desempenho por linha.                        |
|               -c < caminho [caminho []] >               |                                                              Especifica os contadores de desempenho a serem coletados.                                                               |
|                   -SC <value>                    |                                      Especifica o número máximo de amostras a serem coletadas com um coletor de dados de contador de desempenho.                                      |

## <a name="remarks"></a>Comentários  
Onde [-] está listado, um extra-nega a opção.  
## <a name="BKMK_examples"></a>Disso  
O comando a seguir atualiza o perf_log do coletor de dados, alterando o intervalo de amostragem para 10 e o formato de log para CSV e adicionando o controle de versão ao nome do arquivo de log no formato mmddhhmm.  
```  
logman update perf_log -si 10 -f csv -v mmddhhmm  
```  
#### <a name="additional-references"></a>Referências adicionais  
[logman](logman.md)  
[criar contador de logman](logman-create-counter.md)  
