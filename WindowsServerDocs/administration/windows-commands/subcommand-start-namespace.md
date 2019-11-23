---
title: Início do subcomando-namespace
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2dd1c11e-6ab7-4129-9e3a-3f80e0ba59c0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 55fe4a6136fe4f8e886dc62fff746a1e5ff1898f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370753"
---
# <a name="subcommand-start-namespace"></a>Subcomando: Start-namespace

> Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, o Windows Server 2012 inicia um namespace de conversão agendada.
> ## <a name="syntax"></a>Sintaxe
> ```
> wdsutil /start-Namespace /Namespace:<Namespace name> [/Server:<Server name>]
> ```
> ## <a name="parameters"></a>Parâmetros
> 
> |          Parâmetro          |                                                                                                                                                                                             Descrição                                                                                                                                                                                             |
> |-----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | /Namespace:<Namespace name> | Especifica o nome do namespace. Observe que esse não é o nome amigável e deve ser exclusivo.<br /><br />-   **servidor de implantação**: a sintaxe para o nome do namespace é/NAMSPACE: WDS:<Image group>/<Image name>/.<Index> Por exemplo: **WDS: ImageGroup1/install. wim/1**<br />**servidor de transporte**de -   : esse nome deve corresponder ao nome fornecido para o namespace quando ele foi criado no servidor. |
> |   [/Server:<Server name>]   |                                                                                                           Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.                                                                                                           |
> 
> ## <a name="BKMK_examples"></a>Disso
> Para iniciar um namespace, digite um dos seguintes:
> ```
> wdsutil /start-Namespace /Namespace:"Custom Auto 1"
> wdsutil /start-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1"
> ```
> #### <a name="additional-references"></a>referências adicionais
> A [chave de sintaxe de linha de comando](command-line-syntax-key.md)
> [usando o comando Get-mynamespaces](using-the-get-allnamespaces-command.md)
> [usando o comando New-namespace](using-the-new-namespace-command.md)
> [usando o comando Remove-namespace](using-the-remove-namespace-command.md)
