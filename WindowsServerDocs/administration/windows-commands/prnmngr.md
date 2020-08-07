---
title: prnmngr
description: Artigo de referência para o comando Prnmngr, que adiciona, exclui e lista impressoras ou conexões de impressora, além de definir e exibir a impressora padrão.
ms.topic: article
ms.assetid: 39eee1a8-4b41-4c9f-941e-486495135eb8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 53c49de622b38efc6e536d8113b58b43ffef2dbe
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87884736"
---
# <a name="prnmngr"></a>prnmngr

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Adiciona, exclui e lista as impressoras ou conexões de impressora, além de definir e exibir a impressora padrão. Esse comando é um script Visual Basic localizado no `%WINdir%\System32\printing_Admin_Scripts\<language>` diretório. Para usar esse comando em um prompt de comando, digite **cscript** seguido pelo caminho completo para o arquivo Prnmngr ou altere os diretórios para a pasta apropriada. Por exemplo: `cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnmngr`.

## <a name="syntax"></a>Sintaxe

```
cscript prnmngr {-a | -d | -x | -g | -t | -l | -?}[c] [-s <Servername>] [-p <Printername>] [-m <printermodel>] [-r <portname>] [-u <Username>]
[-w <password>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| -a | Adiciona uma conexão de impressora local. |
| -d | Exclui uma conexão de impressora. |
| -X | Exclui todas as impressoras do servidor especificado pelo parâmetro **-s** . Se você não especificar um servidor, o Windows excluirá todas as impressoras no computador local. |
| -g | Exibe a impressora padrão. |
| -T | Define a impressora padrão para a impressora especificada pelo parâmetro **-p** . |
| -l | Lista todas as impressoras instaladas no servidor especificado pelo parâmetro **-s** . Se você não especificar um servidor, o Windows listará as impressoras instaladas no computador local. |
| c | Especifica que o parâmetro se aplica a conexões de impressora. Pode ser usado com os parâmetros **-a** e **-x** . |
| -s`<Servername>` | Especifica o nome do computador remoto que hospeda a impressora que você deseja gerenciar. Se você não especificar um computador, o computador local será usado. |
| -p`<Printername>` | Especifica o nome da impressora que você deseja gerenciar. |
| -m`<Modelname>` | Especifica (por nome) o driver que você deseja instalar. Os drivers geralmente são nomeados para o modelo de impressora para os quais dão suporte. Consulte a documentação da impressora para obter mais informações. |
| -r`<portname>` | Especifica a porta onde a impressora está conectada. Se essa for uma porta paralela ou serial, use a ID da porta (por exemplo, LPT1: ou COM1:). Se esta for uma porta TCP/IP, use o nome da porta que foi especificado quando a porta foi adicionada. |
| -u `<Username>` -w`<password>` | Especifica uma conta com permissões para se conectar ao computador que hospeda a impressora que você deseja gerenciar. Todos os membros do grupo de administradores locais do computador de destino têm essas permissões, mas as permissões também podem ser concedidas a outros usuários. Se você não especificar uma conta, deverá estar conectado sob uma conta com essas permissões para que o comando funcione. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Se as informações fornecidas contiverem espaços, use aspas ao contrário do texto (por exemplo, "nome do computador").

### <a name="examples"></a>Exemplos

Para adicionar uma impressora chamada colorprinter_2 que está conectada a LPT1 no computador local e requer um driver de impressora chamado Color Printer Driver1, digite:

```
cscript prnmngr -a -p colorprinter_2 -m "color printer Driver1" -r lpt1:
```

Para excluir a impressora denominada colorprinter_2 do computador remoto chamado HRServer, digite:

```
cscript prnmngr -d -s HRServer -p colorprinter_2
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Referência aos comandos de impressão](print-command-reference.md)
