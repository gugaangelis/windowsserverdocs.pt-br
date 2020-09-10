---
title: Conjunto de subcomandos-grupo de imagens
description: Artigo de referência para subcomando set-FileGroup, que altera os atributos de um grupo de imagens.
ms.topic: reference
ms.assetid: 4d86946a-e261-4d41-8b0c-1ab0ba2e3430
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 19a4ed7b8dff69a6fdba5d97e6c9c56e44fe4a06
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640848"
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
|[/Name: <New image group name> ]|Especifica o novo nome do grupo de imagens.|
|[/Security: <SDDL> ]|Especifica o novo descritor de segurança do grupo de imagens, no formato SDDL (Security Descriptor Definition Language).|
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
- Chave de sintaxe [de linha de comando](command-line-syntax-key.md) 
 [Usando o comando](using-the-add-imagegroup-command.md) 
 Add-imageus [Usando o comando](using-the-get-allimagegroups-command.md) 
 Get-AllImageGroups [Usando o comando](using-the-get-imagegroup-command.md) 
 Get-imageus [Usando o comando Remove-MyImage](using-the-remove-imagegroup-command.md)
