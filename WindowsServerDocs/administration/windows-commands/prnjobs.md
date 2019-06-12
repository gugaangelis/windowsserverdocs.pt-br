---
title: prnjobs
description: Saiba como gerenciar trabalhos de impressão da linha de comando.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 5e9e71a21acac73aa27e8a936360c6a1e9f9b754
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436159"
---
# <a name="prnjobs"></a>prnjobs

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

faz uma pausa, continua, cancela e lista trabalhos de impressão.

## <a name="syntax"></a>Sintaxe
```
cscript Prnjobs {-z | -m | -x | -l | -?} [-s <ServerName>] 
[-p <printerName>] [-j <JobID>] [-u <UserName>] [-w <Password>]
```

## <a name="parameters"></a>Parâmetros

|          Parâmetro           |                                                                                                                                                                                        Descrição                                                                                                                                                                                        |
|------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              -z              |                                                                                                                                                                 pausa o trabalho de impressão especificado com o **-j** parâmetro.                                                                                                                                                                 |
|              -m              |                                                                                                                                                                Retoma o trabalho de impressão especificado com o **-j** parâmetro.                                                                                                                                                                 |
|              -x              |                                                                                                                                                                Cancela o trabalho de impressão especificado com o **-j** parâmetro.                                                                                                                                                                 |
|              -l              |                                                                                                                                                                        lista todos os trabalhos de impressão em uma fila de impressão.                                                                                                                                                                         |
|       -s \<ServerName>       |                                                                                                                  Especifica o nome do computador remoto que hospeda a impressora que você deseja gerenciar. Se você não especificar um computador, o computador local será usado.                                                                                                                  |
|      -p \<printerName>       |                                                                                                                                                           Especifica o nome da impressora que você deseja gerenciar. Obrigatório.                                                                                                                                                            |
|         -j \<JobID>          |                                                                                                                                                                Especifica que deseja cancelar o trabalho de impressão (por número de identificação).                                                                                                                                                                 |
| -u \<nome de usuário > -w <Password> | Especifica uma conta com permissões para se conectar ao computador que hospeda a impressora que você deseja gerenciar. Todos os membros do grupo de administradores local do computador de destino têm essas permissões, mas também podem ser concedidas as permissões a outros usuários. Se você não especificar uma conta, você deve fazer logon em uma conta com essas permissões para o comando funcione. |
|              /?              |                                                                                                                                                                           Exibe a ajuda no prompt de comando.                                                                                                                                                                            |

## <a name="remarks"></a>Comentários
-   O **prnjobs** comando é um script do Visual Basic localizado no %WINdir%\System32\printing_Admin_Scripts\\ <language> directory. Para usar este comando no prompt de comando, digite **cscript** seguido pelo caminho completo do arquivo prnjobs ou altere os diretórios para a pasta apropriada. Por exemplo:
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnjobs.vbs
    ```
-   Se as informações que você fornece contiverem espaços, use aspas ao redor do texto (por exemplo, `"computer Name"`).

## <a name="BKMK_examples"></a>Exemplos
Para pausar um trabalho de impressão com uma ID de trabalho de 27 enviadas para o computador remoto Servidor_HR para impressão em Impressora_colorida, digite:
```
cscript prnjobs.vbs -z -s HRServer -p colorprinter -j 27
```
Para listar todos os trabalhos de impressão atuais na fila para a impressora local chamada Impressora_colorida_2, digite:
```
cscript prnjobs.vbs -l -p colorprinter_2
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Referência aos comandos de impressão](print-command-reference.md)
