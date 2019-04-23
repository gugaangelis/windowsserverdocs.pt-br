---
title: Usando o comando get-servidor
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: da8bf0fc6e31bd8d0079933f1d7c529c4fe96f42
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870977"
---
# <a name="using-the-get-server-command"></a>Usando o comando get-servidor

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera informações do servidor dos serviços de implantação do Windows especificado.
## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Get-Server [/Server:<Server name>] /Show:{Config | Images | All} [/detailed]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Isso pode ser o nome NetBIOS ou o nome de domínio totalmente qualificado (FQDN). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|/ Mostrar: {Config &#124; imagens &#124; todos os}|Especifica o tipo de informação a ser retornada.<br /><br />-   **Config** retorna informações de configuração.<br />-   **Imagens** retorna informações sobre grupos de imagens, imagens de inicialização e as imagens de instalação.<br />-   **Todos os** retorna informações de configuração e informações da imagem.|
|[/ detalhadas]|Você pode usar essa opção com **/Show:Images** ou **/Show:All** para indicar que todos os metadados de imagem de cada imagem devem ser retornado. Se o **/ detalhadas** opção não for usada, o comportamento padrão é retornar o nome da imagem, descrição e nome de arquivo.|
## <a name="BKMK_examples"></a>Exemplos
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
[usando o comando de servidor disable](using-the-disable-server-command.md)
[usando o comando enable-Server](using-the-enable-server-command.md)
[usando o Comando do servidor de inicialização](using-the-initialize-server-command.md)
[subcomando: set-Server](subcommand-set-server.md)
[subcomando: Iniciar servidor](subcommand-start-server.md) 
 [ Subcomando: stop-Server](subcommand-stop-server.md)
[o opção de servidor uninitialize](the-uninitialize-server-option.md)
