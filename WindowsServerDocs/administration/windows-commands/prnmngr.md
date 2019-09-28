---
title: prnmngr
description: Saiba como adicionar, excluir e listar impressoras e conexões.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 12981519a1d3bfc079a58e5883bc845955b8a8c6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372075"
---
# <a name="prnmngr"></a>prnmngr

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Adiciona, exclui e lista as impressoras ou conexões de impressora, além de definir e exibir a impressora padrão.

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
|              -d               |                                                                                                                                                                               exclui uma conexão de impressora.                                                                                                                                                                               |
|              -x               |                                                                                                               exclui todas as impressoras do servidor especificado com o parâmetro **-s** . Se você não especificar um servidor, o Windows excluirá todas as impressoras no computador local.                                                                                                               |
|              -g               |                                                                                                                                                                               Exibe a impressora padrão.                                                                                                                                                                               |
|              -t               |                                                                                                                                                        Define a impressora padrão para a impressora especificada pelo parâmetro **-p** .                                                                                                                                                         |
|              -l               |                                                                                                         lista todas as impressoras instaladas no servidor especificado pelo parâmetro **-s** . Se você não especificar um servidor, o Windows listará as impressoras instaladas no computador local.                                                                                                         |
|               c               |                                                                                                                                      Especifica que o parâmetro se aplica a conexões de impressora. Pode ser usado com os parâmetros **-a** e **-x** .                                                                                                                                      |
|        -s <ServerName>        |                                                                                                                  Especifica o nome do computador remoto que hospeda a impressora que você deseja gerenciar. Se você não especificar um computador, o computador local será usado.                                                                                                                  |
|       -p \<printerName >       |                                                                                                                                                                Especifica o nome da impressora que você deseja gerenciar.                                                                                                                                                                 |
|     -m \<DrivermodelName >     |                                                                                                          Especifica (por nome) o driver que você deseja instalar. Os drivers geralmente são nomeados para o modelo de impressora para os quais dão suporte. Consulte a documentação da impressora para obter mais informações.                                                                                                           |
|        -r \<PortName >         |                                                                         Especifica a porta onde a impressora está conectada. Se essa for uma porta paralela ou serial, use a ID da porta (por exemplo, LPT1: ou COM1:). Se esta for uma porta TCP/IP, use o nome da porta que foi especificado quando a porta foi adicionada.                                                                          |
| -u \<UserName >-w \<Password > | Especifica uma conta com permissões para se conectar ao computador que hospeda a impressora que você deseja gerenciar. Todos os membros do grupo de administradores locais do computador de destino têm essas permissões, mas as permissões também podem ser concedidas a outros usuários. Se você não especificar uma conta, deverá estar conectado sob uma conta com essas permissões para que o comando funcione. |
|              /?               |                                                                                                                                                                           Exibe a ajuda no prompt de comando.                                                                                                                                                                            |

## <a name="remarks"></a>Comentários
-   O comando **prndrvr** é um script Visual Basic localizado no diretório%WINdir%\System32\printing_Admin_Scripts @ no__t-1 @ no__t-2. Para usar esse comando, em um prompt de comando, digite **cscript** seguido pelo caminho completo para o arquivo **prnmngr** ou altere os diretórios para a pasta apropriada. Por exemplo:
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnmngr
    ```
-   Se as informações fornecidas contiverem espaços, use aspas ao contrário do texto (por exemplo, `"computer Name"`).

## <a name="BKMK_examples"></a>Disso
Para adicionar uma impressora chamada Impressora_colorida_2 que está conectada a LPT1 no computador local e requer um driver de impressora chamado Color Printer Driver1, digite:
```
cscript prnmngr -a -p colorprinter_2 -m "color printer Driver1" -r lpt1:
```
Para excluir a impressora chamada Impressora_colorida_2 do computador remoto chamado HRServer, digite:
```
cscript prnmngr -d -s HRServer -p colorprinter_2 
```

#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[referência de comando de impressão](print-command-reference.md)
