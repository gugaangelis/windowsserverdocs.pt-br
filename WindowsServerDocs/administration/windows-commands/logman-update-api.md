---
title: API de atualização do logman
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6f322e52-0f9f-42b1-bd64-8b8f8fe086fc britw
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 42ee594bfb4578cebec062a5c2a81d11dae81349
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724311"
---
# <a name="logman-update-api"></a>API de atualização do logman

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Atualize as propriedades de um coletor de dados de rastreamento de API existente.  

## <a name="syntax"></a>Sintaxe  
```  
logman update api <[-n] <name>> [options]  
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
|            -mods <caminho [caminho [...]] >             |                                                          Especifica a lista de módulos da qual registrar chamadas de API.                                                           |
|     -inapis <módulo! API [módulo! API [...]] >      |                                                         Especifica a lista de chamadas de API a serem incluídas no registro em log.                                                          |
|     -exapis <módulo! API [módulo! API [...]] >      |                                                        Especifica a lista de chamadas de API a serem excluídas do registro em log.                                                         |
|                     -[-] ano                      |                                                     Somente nomes de API de log (-ano) ou não registram somente os nomes de API (-ano).                                                     |
|                  -[-] recursivo                   |                                          Registrar (-recursivo) ou não registrar (-recursivo) APIs recursivamente além da primeira camada.                                           |
|                   -exe <value>                   |                                                        Especifica o caminho completo para um executável para rastreamento de API.                                                        |

## <a name="remarks"></a>Comentários  
Onde [-] está listado, um extra-nega a opção.  
## <a name="examples"></a>Exemplos  
O comando a seguir atualiza o contador de rastreamento de API existente chamado trace_notepad para o arquivo executável c:\Windows\Notepad.exe, excluindo a chamada de API TlsGetValue produzida pelo módulo Kernel32. dll.  
```  
logman create api trace_notepad -exe c:\windows\notepad.exe -exapis kernel32.dll!TlsGetValue  
```  
## <a name="additional-references"></a>Referências adicionais  
[logman](logman.md)  
[criar API do logman](logman-create-api.md)  
