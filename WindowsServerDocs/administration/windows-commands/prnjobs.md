---
title: prnjobs
description: Saiba como gerenciar trabalhos de impressão na linha de comando.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5ad34199-7a5a-40c1-8053-bccd5929df43
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: c4fb9be9545274bbbf33926042f7a4deec5ceb05
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372101"
---
# <a name="prnjobs"></a>prnjobs

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Pausa, retoma, cancela e lista trabalhos de impressão.

## <a name="syntax"></a>Sintaxe
```
cscript Prnjobs {-z | -m | -x | -l | -?} [-s <ServerName>] 
[-p <printerName>] [-j <JobID>] [-u <UserName>] [-w <Password>]
```

## <a name="parameters"></a>Parâmetros

|          Parâmetro           |                                                                                                                                                                                        Descrição                                                                                                                                                                                        |
|------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              -z              |                                                                                                                                                                 pausa o trabalho de impressão especificado com o parâmetro **-j** .                                                                                                                                                                 |
|              -m              |                                                                                                                                                                Retoma o trabalho de impressão especificado com o parâmetro **-j** .                                                                                                                                                                 |
|              -x              |                                                                                                                                                                Cancela o trabalho de impressão especificado com o parâmetro **-j** .                                                                                                                                                                 |
|              -l              |                                                                                                                                                                        lista todos os trabalhos de impressão em uma fila de impressão.                                                                                                                                                                         |
|       -s \<ServerName >       |                                                                                                                  Especifica o nome do computador remoto que hospeda a impressora que você deseja gerenciar. Se você não especificar um computador, o computador local será usado.                                                                                                                  |
|      -p \<PrinterName >       |                                                                                                                                                           Especifica o nome da impressora que você deseja gerenciar. Necessário.                                                                                                                                                            |
|         -j \<JobID >          |                                                                                                                                                                Especifica (por número de ID) o trabalho de impressão que você deseja cancelar.                                                                                                                                                                 |
| -u \<nome de usuário >-w <Password> | Especifica uma conta com permissões para se conectar ao computador que hospeda a impressora que você deseja gerenciar. Todos os membros do grupo de administradores locais do computador de destino têm essas permissões, mas as permissões também podem ser concedidas a outros usuários. Se você não especificar uma conta, deverá estar conectado sob uma conta com essas permissões para que o comando funcione. |
|              /?              |                                                                                                                                                                           Exibe a ajuda no prompt de comando.                                                                                                                                                                            |

## <a name="remarks"></a>Comentários
-   O comando **prnjobs** é um script Visual Basic localizado no diretório <language> do printing_Admin_Scripts\\do%windir%\system32\. Para usar esse comando, em um prompt de comando, digite **cscript** seguido pelo caminho completo para o arquivo prnjobs ou altere os diretórios para a pasta apropriada. Por exemplo:
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnjobs.vbs
    ```
-   Se as informações fornecidas contiverem espaços, use aspas ao contrário do texto (por exemplo, `"computer Name"`).

## <a name="BKMK_examples"></a>Disso
Para pausar um trabalho de impressão com uma ID de trabalho de 27 enviada para o computador remoto chamado HRServer para impressão na impressora denominada colorprinter, digite:
```
cscript prnjobs.vbs -z -s HRServer -p colorprinter -j 27
```
Para listar todos os trabalhos de impressão atuais na fila para a impressora local denominada colorprinter_2, digite:
```
cscript prnjobs.vbs -l -p colorprinter_2
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Referência aos comandos de impressão](print-command-reference.md)
