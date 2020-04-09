---
title: Get-Server
description: O tópico de comandos do Windows para Get-Server, que recupera informações do servidor de serviços de implantação do Windows especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bef60db4-d58d-4304-ab4b-be53dd3271c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 65903dc89730eb9d1da23be31ecc1909daece9c4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830839"
---
# <a name="get-server"></a>Get-Server

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera informações do servidor de serviços de implantação do Windows especificado.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Get-Server [/Server:<Server name>] /Show:{Config | Images | All} [/detailed]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Esse pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|/Show: {config &#124; images &#124; All}|Especifica o tipo de informações a serem retornadas.<p>-   **config** retorna informações de configuração.<br />-   **imagens** retorna informações sobre grupos de imagens, imagens de inicialização e imagens de instalação.<br />-   **todos** retorna informações de configuração e informações de imagem.|
|[/detailed]|Você pode usar essa opção com **/show: images** ou **/show: ALL** para indicar que todos os metadados de imagem de cada imagem devem ser retornados. Se a opção **/detailed** não for usada, o comportamento padrão será retornar o nome da imagem, a descrição e o nome do arquivo.|
## <a name="examples"></a><a name=BKMK_examples></a>Disso
Para exibir informações sobre o servidor, digite:
```
wdsutil /Get-Server /Show:Config
```
Para exibir informações detalhadas sobre o servidor, digite:
```
wdsutil /verbose /Get-Server /Server:MyWDSServer /Show:All /detailed
```
## <a name="additional-references"></a>Referências adicionais
- A [chave de sintaxe de linha de comando](command-line-syntax-key.md)
usando o [comando disable-Server](using-the-disable-server-command.md)
[usando o comando Enable-Server](using-the-enable-server-command.md)
[usando o comando Initialize-Server](using-the-initialize-server-command.md)
subcomando [: Set-Server](subcommand-set-server.md)
[subcomando: Start-Server](subcommand-start-server.md)
[Subcommand: Stop-Server](subcommand-stop-server.md)
[a opção não Initialize-Server](the-uninitialize-server-option.md)
