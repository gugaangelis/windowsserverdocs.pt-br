---
title: mstsc
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 59801227-1e7e-4dbd-96e6-f54102a3ce92
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b6f89c1e3b0d36f14dbd55f9e6994c788305b30d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437180"
---
# <a name="mstsc"></a>mstsc

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria conexões com servidores de Host de sessão de área de trabalho remota (Host de sessão rd) ou outros computadores remotos, edita um arquivo de configuração de Conexão de área de trabalho remota (. rdp) existente e migra os arquivos de conexão herdados que foram criados com o Gerenciador de Conexão de cliente para novos arquivos de conexão. rdp.
Para obter exemplos de como usar esse comando, consulte [exemplos](#BKMK_examples).
> [!NOTE]
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir o que há de novo na versão mais recente, consulte [novidades novo nos serviços de área de trabalho remota no Windows Server 2012](https://technet.microsoft.com/library/hh831527) na biblioteca do TechNet do Windows Server.

## <a name="syntax"></a>Sintaxe
```
mstsc.exe [<Connection File>] [/v:<Server>[:<Port>]] [/admin] [/f] [/w:<Width> /h:<Height>] [/public] [/span]
mstsc.exe /edit <Connection File>
mstsc.exe /migrate
```

## <a name="parameters"></a>Parâmetros

|        Parâmetro        |                                                         Descrição                                                         |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------|
|    <Connection File>    |                                   Especifica o nome de um arquivo. rdp para a conexão.                                    |
|   /v:<Server[:<Port>]   |                Especifica o computador remoto e, opcionalmente, o número da porta à qual você deseja se conectar.                 |
|         /admin          |                                   Você se conecta a uma sessão para administrar o servidor.                                   |
|           /f            |                                    Inicia a Conexão de área de trabalho remota no modo de tela inteira.                                    |
|       /w:<Width>        |                                      Especifica a largura da janela de área de trabalho remota.                                      |
|       /h:<Height>       |                                     Especifica a altura da janela de área de trabalho remota.                                      |
|         / público         |                  Executa a área de trabalho remota no modo público. No modo público, não estão em cache as senhas e bitmaps.                  |
|          /span          | Corresponde a largura da área de trabalho remota e a altura com o desktop virtual local, abrangendo em vários monitores, se necessário. |
| /Edit <Connection File> |                                         Abre o arquivo. rdp especificado para edição.                                          |
|        / migrar         |       Migra os arquivos de conexão herdados que foram criados com o Gerenciador de Conexão de cliente para novos arquivos de conexão. rdp.       |
|           /?            |                                            Exibe a ajuda no prompt de comando.                                             |

## <a name="remarks"></a>Comentários
-   Default é armazenado para cada usuário como um arquivo oculto na pasta de documentos do usuário. Usuário que criou os arquivos. rdp são salvos por padrão na pasta de documentos do usuário, mas podem ser salvos em qualquer lugar.
-   Para abranger os monitores, os monitores devem ter a mesma resolução e devem estar alinhados horizontalmente (ou seja, lado a lado). Atualmente, não há nenhum suporte para vários monitores verticalmente no sistema cliente.

## <a name="BKMK_examples"></a>Exemplos
-   Para se conectar a uma sessão no modo de tela inteira, digite:
    ```
    mstsc /f
    ```
-   Para abrir um arquivo chamado filename para edição, digite:
    ```
    mstsc /edit filename.rdp
    ```

#### <a name="additional-references"></a>Referências adicionais
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Serviços de área de trabalho remota &#40;serviços de Terminal&#41; referência do comando](remote-desktop-services-terminal-services-command-reference.md)
