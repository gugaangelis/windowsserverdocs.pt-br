---
title: Usando o comando Get-Server
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bef60db4-d58d-4304-ab4b-be53dd3271c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c7e0ee4529858b16cdc63ea1d6d358a190b8b1a9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392112"
---
# <a name="using-the-get-server-command"></a>Usando o comando Get-Server

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera informações do servidor de serviços de implantação do Windows especificado.
## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Get-Server [/Server:<Server name>] /Show:{Config | Images | All} [/detailed]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Esse pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|/Show: {config &#124; images &#124; All}|Especifica o tipo de informações a serem retornadas.<br /><br />a**configuração -    retorna** informações de configuração.<br />as**imagens** -    retornam informações sobre grupos de imagens, imagens de inicialização e imagens de instalação.<br />-   **All** retorna informações de configuração e informações de imagem.|
|[/detailed]|Você pode usar essa opção com **/show: images** ou **/show: ALL** para indicar que todos os metadados de imagem de cada imagem devem ser retornados. Se a opção **/detailed** não for usada, o comportamento padrão será retornar o nome da imagem, a descrição e o nome do arquivo.|
## <a name="BKMK_examples"></a>Disso
Para exibir informações sobre o servidor, digite:
```
wdsutil /Get-Server /Show:Config
```
Para exibir informações detalhadas sobre o servidor, digite:
```
wdsutil /verbose /Get-Server /Server:MyWDSServer /Show:All /detailed
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando disable-Server](using-the-disable-server-command.md)
[usando o comando Enable-Server](using-the-enable-server-command.md)
[usando o comando Initialize-Server](using-the-initialize-server-command.md)
[subcomando: Set-Server](subcommand-set-server.md)
[ Subcomando: Start-Server](subcommand-start-server.md)1[subcomando: Stop-Server](subcommand-stop-server.md)3[a opção Uninitialize-Server](the-uninitialize-server-option.md)
