---
title: Conjunto de subcomandos-grupo de imagens
description: Tópico de referência para subcomando set-FileGroup, que altera os atributos de um grupo de imagens.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4d86946a-e261-4d41-8b0c-1ab0ba2e3430
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 429a8fee5b0236d264eb421f110219a1bc037368
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721705"
---
# <a name="subcommand-set-imagegroup"></a>Subcomando: Set-grupo de imagens

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera os atributos de um grupo de imagens.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Set-ImageGroumediaGroup:<Image group name> [/Server:<Server name>] [/Name:<New image group name>] [/Security:<SDDL>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
The Media:<Image group name>|Especifica o nome do grupo de imagens.|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se não for especificado, o servidor local será usado.|
|[/Name:<New image group name>]|Especifica o novo nome do grupo de imagens.|
|[/Security:<SDDL>]|Especifica o novo descritor de segurança do grupo de imagens, no formato SDDL (Security Descriptor Definition Language).|
## <a name="examples"></a>Exemplos
Para definir o nome de um grupo de imagens, digite:
```
wdsutil /Set-ImageGroumediaGroup:ImageGroup1 /Name:New Image Group Name
```
Para especificar várias configurações para um grupo de imagens, digite:
```
wdsutil /verbose /Set-ImageGroumediaGroup:ImageGroup1 /Server:MyWDSServer /Name:New Image Group Name 
/Security:O:BAG:S-1-5-21-2176941838-3499754553-4071289181-513 D:AI(A;ID;FA;;;SY)(A;OICIIOID;GA;;;SY)(A;ID;FA;;;BA)(A;OICIIOID;GA;;;BA) (A;ID;0x1200a9;;;AU)(A;OICIIOID;GXGR;;;AU)
```
## <a name="additional-references"></a>Referências adicionais
- [Chave](command-line-syntax-key.md)
de sintaxe de linha de comando[usando o comando](using-the-add-imagegroup-command.md)
Add-Image, usando o comando[Get-AllImageGroups](using-the-get-allimagegroups-command.md)
[usando o comando](using-the-get-imagegroup-command.md)
Get-imageus[usando o comando Remove-Image-](using-the-remove-imagegroup-command.md)
