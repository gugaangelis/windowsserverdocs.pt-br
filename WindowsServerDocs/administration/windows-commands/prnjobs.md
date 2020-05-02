---
title: prnjobs
description: Saiba como gerenciar trabalhos de impressão na linha de comando.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5ad34199-7a5a-40c1-8053-bccd5929df43
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: a40a8cbc3c8b13c99cc440b8de797898a5a6249b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722836"
---
# <a name="prnjobs"></a>prnjobs

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Pausa, retoma, cancela e lista trabalhos de impressão.

## <a name="syntax"></a>Sintaxe
```
cscript Prnjobs {-z | -m | -x | -l | -?} [-s <ServerName>] 
[-p <printerName>] [-j <JobID>] [-u <UserName>] [-w <Password>]
```

### <a name="parameters"></a>Parâmetros

|          Parâmetro           |                                                                                                                                                                                        Descrição                                                                                                                                                                                        |
|------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              -Z              |                                                                                                                                                                 pausa o trabalho de impressão especificado com o parâmetro **-j** .                                                                                                                                                                 |
|              -M              |                                                                                                                                                                Retoma o trabalho de impressão especificado com o parâmetro **-j** .                                                                                                                                                                 |
|              -X              |                                                                                                                                                                Cancela o trabalho de impressão especificado com o parâmetro **-j** .                                                                                                                                                                 |
|              -l              |                                                                                                                                                                        lista todos os trabalhos de impressão em uma fila de impressão.                                                                                                                                                                         |
|       -s \<servername>       |                                                                                                                  Especifica o nome do computador remoto que hospeda a impressora que você deseja gerenciar. Se você não especificar um computador, o computador local será usado.                                                                                                                  |
|      -p \<printername>       |                                                                                                                                                           Especifica o nome da impressora que você deseja gerenciar. Obrigatórios.                                                                                                                                                            |
|         -j \<JobID>          |                                                                                                                                                                Especifica (por número de ID) o trabalho de impressão que você deseja cancelar.                                                                                                                                                                 |
| -u \<nome de usuário>-w<Password> | Especifica uma conta com permissões para se conectar ao computador que hospeda a impressora que você deseja gerenciar. Todos os membros do grupo de administradores locais do computador de destino têm essas permissões, mas as permissões também podem ser concedidas a outros usuários. Se você não especificar uma conta, deverá estar conectado sob uma conta com essas permissões para que o comando funcione. |
|              /?              |                                                                                                                                                                           Exibe a ajuda no prompt de comando.                                                                                                                                                                            |

## <a name="remarks"></a>Comentários
-   O comando **prnjobs** é um script Visual Basic localizado no diretório%windir%\system32\ printing_Admin_Scripts\\ <language> . Para usar esse comando, em um prompt de comando, digite **cscript** seguido pelo caminho completo para o arquivo prnjobs ou altere os diretórios para a pasta apropriada. Por exemplo: 
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnjobs.vbs
    ```
-   Se as informações fornecidas contiverem espaços, use aspas em volta do texto (por exemplo, `"computer Name"`).

## <a name="examples"></a><a name="BKMK_examples"></a>Disso
Para pausar um trabalho de impressão com uma ID de trabalho de 27 enviada para o computador remoto chamado HRServer para impressão na impressora denominada colorprinter, digite:
```
cscript prnjobs.vbs -z -s HRServer -p colorprinter -j 27
```
Para listar todos os trabalhos de impressão atuais na fila para a impressora local denominada colorprinter_2, digite:
```
cscript prnjobs.vbs -l -p colorprinter_2
```

## <a name="additional-references"></a>Referências adicionais

-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Referência aos comandos de impressão](print-command-reference.md)
