---
title: prnmngr
description: Saiba como adicionar, excluir e listar as impressoras e conexões.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 39eee1a8-4b41-4c9f-941e-486495135eb8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: f0e3af7b05b77400d3d8a04d048b34b8c553438d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436235"
---
# <a name="prnmngr"></a>prnmngr

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Adiciona, exclui e lista impressoras ou conexões de impressora, além de definir e exibir a impressora padrão.

## <a name="syntax"></a>Sintaxe
```
cscript Prnmngr {-a | -d | -x | -g | -t | -l | -?}[c] [-s <ServerName>] 
[-p <printerName>] [-m <printermodel>] [-r <PortName>] [-u <UserName>] 
[-w <Password>]
```

## <a name="parameters"></a>Parâmetros

|           Parâmetro           |                                                                                                                                                                                        Descrição                                                                                                                                                                                        |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              -a               |                                                                                                                                                                             Adiciona uma conexão de impressora local.                                                                                                                                                                              |
|              -d               |                                                                                                                                                                               Exclui uma conexão de impressora.                                                                                                                                                                               |
|              -x               |                                                                                                               Exclui todas as impressoras do servidor especificado com o **-s** parâmetro. Se você não especificar um servidor, o Windows exclui todas as impressoras no computador local.                                                                                                               |
|              -g               |                                                                                                                                                                               Exibe a impressora padrão.                                                                                                                                                                               |
|              -t               |                                                                                                                                                        Define a impressora padrão para a impressora especificada pelo **-p** parâmetro.                                                                                                                                                         |
|              -l               |                                                                                                         lista todas as impressoras instaladas no servidor especificado o **-s** parâmetro. Se você não especificar um servidor, o Windows lista as impressoras instaladas no computador local.                                                                                                         |
|               c               |                                                                                                                                      Especifica que o parâmetro se aplica a conexões de impressora. Pode ser usado com o **- a** e **- x** parâmetros.                                                                                                                                      |
|        -s <ServerName>        |                                                                                                                  Especifica o nome do computador remoto que hospeda a impressora que você deseja gerenciar. Se você não especificar um computador, o computador local será usado.                                                                                                                  |
|       -p \<printerName>       |                                                                                                                                                                Especifica o nome da impressora que você deseja gerenciar.                                                                                                                                                                 |
|     -m \<DrivermodelName>     |                                                                                                          Especifica o driver que você deseja instalar (por nome). Drivers geralmente são nomeados para o modelo de impressora para que dar suporte a eles. Consulte a documentação da impressora para obter mais informações.                                                                                                           |
|        -r \<PortName>         |                                                                         Especifica a porta em que a impressora está conectada. Se esse for um paralelo ou uma porta serial, use a ID da porta (por exemplo, LPT1: COM1:). Quando se trata de uma porta TCP/IP, use o nome da porta que foi especificado quando a porta foi adicionada.                                                                          |
| -u \<nome de usuário > -w \<senha > | Especifica uma conta com permissões para se conectar ao computador que hospeda a impressora que você deseja gerenciar. Todos os membros do grupo de administradores local do computador de destino têm essas permissões, mas também podem ser concedidas as permissões a outros usuários. Se você não especificar uma conta, você deve fazer logon em uma conta com essas permissões para o comando funcione. |
|              /?               |                                                                                                                                                                           Exibe a ajuda no prompt de comando.                                                                                                                                                                            |

## <a name="remarks"></a>Comentários
-   O **prndrvr** comando é um script do Visual Basic localizado no %WINdir%\System32\printing_Admin_Scripts\\ <language> directory. Para usar este comando no prompt de comando, digite **cscript** seguido pelo caminho completo para o **prnmngr** de arquivo, ou altere os diretórios para a pasta apropriada. Por exemplo:
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnmngr
    ```
-   Se as informações que você fornece contiverem espaços, use aspas ao redor do texto (por exemplo, `"computer Name"`).

## <a name="BKMK_examples"></a>Exemplos
Para adicionar uma impressora chamada Impressora_colorida_2 que está conectada a LPT1 no computador local e requer um driver de impressora chamado color printer Driver1, digite:
```
cscript prnmngr -a -p colorprinter_2 -m "color printer Driver1" -r lpt1:
```
Para excluir a impressora nomeada Impressora_colorida_2 do computador remoto Servidor_HR, digite:
```
cscript prnmngr -d -s HRServer -p colorprinter_2 
```

#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[referência do comando Imprimir](print-command-reference.md)
