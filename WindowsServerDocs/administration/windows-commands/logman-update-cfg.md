---
title: cfg de atualização de logman
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9da4e8b4-3be5-42d3-b0b4-c429630c35c4 britw
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c74370432dbc21f244dd675bb62cc65a13fa2ec7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840529"
---
# <a name="logman-update-cfg"></a>cfg de atualização de logman

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Atualize as propriedades de um coletor de dados de configuração existente.  

## <a name="syntax"></a>Sintaxe  
```  
logman update cfg <[-n] <name>> [options]  
```  
### <a name="parameters"></a>Parâmetros  

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
|                      -[-] Ni                      |                                                         Habilite (-Ni) ou desabilite a consulta de interface de rede (-Ni).                                                          |
|             -reg < caminho [caminho [...]] >             |                                                                 Especifica o (s) valor (es) do registro a serem coletados.                                                                 |
|            -< consulta de gerenciamento [consulta [...]] >            |                                                      Especifica o objeto (s) WMI a ser coletado usando a linguagem de consulta SQL.                                                       |
|             -FTC < caminho [caminho [...]] >             |                                                           Especifica o caminho completo para os arquivos a serem coletados.                                                            |

## <a name="remarks"></a>Comentários  
Onde [-] está listado, um extra-nega a opção.  
## <a name="examples"></a><a name=BKMK_examples></a>Disso  
O comando a seguir atualiza o cfg_log do coletor de dados de configuração existente para coletar a chave do registro HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows NT\Currentverion\\.  
```  
logman update cfg cfg_log -reg HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\Currentverion\  
```  
## <a name="additional-references"></a>Referências adicionais  
[logman](logman.md)  
[logman Create cfg](logman-create-cfg.md)  
