---
title: WDSUTIL remover-MyImage
description: Artigo de referência para WDSUTIL Remove-MyImage, que remove um grupo de imagens de um servidor.
ms.topic: reference
ms.assetid: 5b2c9813-5df2-4272-8449-26f3bb16f82b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c9691281a706a2929c7406bfcca2c3f3410112e0
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729815"
---
# <a name="wdsutil-remove-imagegroup"></a>WDSUTIL remover-MyImage

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Remove um grupo de imagens de um servidor.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /remove-ImageGroup Group:<Image group name> [/Server:<Server name>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|MyImage:<Image group name>|Especifica o nome do grupo de imagens a ser removido|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
## <a name="examples"></a>Exemplos
Para remover o grupo de imagens, digite um dos seguintes:
```
wdsutil /remove-ImageGroumediaGroup:ImageGroup1
wdsutil /verbose /remove-ImageGroumediaGroup:My Image Group /Server:MyWDSServer
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [comando de Add-Image do WDSUTIL](wdsutil-add-imagegroup.md)
- [comando WDSUTIL Get-allimagegroups](wdsutil-get-allimagegroups.md)
- [comando WDSUTIL Get-Imageobject](wdsutil-get-imagegroup.md)
- [conjunto WDSUTIL-comando do grupo de imagens](wdsutil-set-imagegroup.md)
