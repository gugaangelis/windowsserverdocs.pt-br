---
title: Conjunto de subcomandos-grupo de imagens
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4d86946a-e261-4d41-8b0c-1ab0ba2e3430
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 10023e493ae4db51783b7401c12bc1605145b86c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370791"
---
# <a name="subcommand-set-imagegroup"></a>Subcomando: Set-grupo de imagens

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

altera os atributos de um grupo de imagens.
## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Set-ImageGroumediaGroup:<Image group name> [/Server:<Server name>] [/Name:<New image group name>] [/Security:<SDDL>]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
The Media: <Image group name>|Especifica o nome do grupo de imagens.|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se não for especificado, o servidor local será usado.|
|[/Name: <New image group name>]|Especifica o novo nome do grupo de imagens.|
|[/Security: <SDDL>]|Especifica o novo descritor de segurança do grupo de imagens, no formato SDDL (Security Descriptor Definition Language).|
## <a name="BKMK_examples"></a>Disso
Para definir o nome de um grupo de imagens, digite:
```
wdsutil /Set-ImageGroumediaGroup:ImageGroup1 /Name:"New Image Group Name"
```
Para especificar várias configurações para um grupo de imagens, digite:
```
wdsutil /verbose /Set-ImageGroumediaGroup:ImageGroup1 /Server:MyWDSServer /Name:"New Image Group Name" 
/Security:"O:BAG:S-1-5-21-2176941838-3499754553-4071289181-513 D:AI(A;ID;FA;;;SY)(A;OICIIOID;GA;;;SY)(A;ID;FA;;;BA)(A;OICIIOID;GA;;;BA) (A;ID;0x1200a9;;;AU)(A;OICIIOID;GXGR;;;AU)"
```
#### <a name="additional-references"></a>Referências adicionais
A [chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando Add-Image do](using-the-add-imagegroup-command.md)
[usando o comando Get-AllImageGroups](using-the-get-allimagegroups-command.md)
[usando o comando Get-Image](using-the-get-imagegroup-command.md)do 
[usando o comando Remove-](using-the-remove-imagegroup-command.md) filedeler
