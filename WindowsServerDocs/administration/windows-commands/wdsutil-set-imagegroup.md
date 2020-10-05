---
title: WDSUTIL conjunto-grupo de imagens
description: Artigo de referência para subcomando set-FileGroup, que altera os atributos de um grupo de imagens.
ms.topic: reference
ms.assetid: 4d86946a-e261-4d41-8b0c-1ab0ba2e3430
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6f3b2b3790ecc126f8be48ade61d305d44011f68
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729831"
---
# <a name="wdsutil-set-imagegroup"></a>WDSUTIL conjunto-grupo de imagens

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera os atributos de um grupo de imagens.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /set-imagegroup:<Image group name> [/Server:<Server name>] [/Name:<New image group name>] [/Security:<SDDL>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/set-imagegroup:<Image group name>|Especifica o nome do grupo de imagens.|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se não for especificado, o servidor local será usado.|
|[/Name: <New image group name> ]|Especifica o novo nome do grupo de imagens.|
|[/Security: <SDDL> ]|Especifica o novo descritor de segurança do grupo de imagens, no formato SDDL (Security Descriptor Definition Language).|
## <a name="examples"></a>Exemplos
Para definir o nome de um grupo de imagens, digite:
```
wdsutil /Set-ImageGroup:ImageGroup1 /Name:New Image Group Name
```
Para especificar várias configurações para um grupo de imagens, digite:
```
wdsutil /verbose /Set-ImageGroupGroup:ImageGroup1 /Server:MyWDSServer /Name:New Image Group Name
/Security:O:BAG:S-1-5-21-2176941838-3499754553-4071289181-513 D:AI(A;ID;FA;;;SY)(A;OICIIOID;GA;;;SY)(A;ID;FA;;;BA)(A;OICIIOID;GA;;;BA) (A;ID;0x1200a9;;;AU)(A;OICIIOID;GXGR;;;AU)
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [comando de Add-Image do WDSUTIL](wdsutil-add-imagegroup.md)
- [comando WDSUTIL Get-allimagegroups](wdsutil-get-allimagegroups.md)
- [comando WDSUTIL Get-Imageobject](wdsutil-get-imagegroup.md)
- [comando de remoção do WDSUTIL-MyImage](wdsutil-remove-imagegroup.md)
