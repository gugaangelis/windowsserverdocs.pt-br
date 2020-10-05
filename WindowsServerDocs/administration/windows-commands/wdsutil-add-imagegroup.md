---
title: WDSUTIL Add-MyImage
description: Artigo de referência do WDSUTIL Add-Image Group, que adiciona um grupo de imagens a um servidor de serviços de implantação do Windows.
ms.topic: reference
ms.assetid: 6ca88671-51de-4924-b969-88f3dfd84270
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 97f1439a4212a770dff6f6c42837c531f08c3d25
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729704"
---
# <a name="wdsutil-add-imagegroup"></a>WDSUTIL Add-MyImage

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Adiciona um grupo de imagens a um servidor dos serviços de implantação do Windows.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /add-ImageGroup imageGroup:<Image group name> [/Server:<Server name>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|MyImage:<Image group name>|Especifica o nome do grupo de imagens a ser adicionado.|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se um nome do servidor não for especificado, o servidor local será usado.|
## <a name="examples"></a>Exemplos
Para adicionar um grupo de imagens, digite um dos seguintes:
```
wdsutil /add-ImageGroup imageGroup:ImageGroup2
wdsutil /verbose /add-Imagegroup imageGroup:My Image Group /Server:MyWDSServer
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [comando WDSUTIL Get-allimagegroups](wdsutil-get-allimagegroups.md)
- [comando WDSUTIL Get-Imageobject](wdsutil-get-imagegroup.md)
- [comando de remoção do WDSUTIL-MyImage](wdsutil-remove-imagegroup.md)
- [conjunto WDSUTIL-comando do grupo de imagens](wdsutil-set-imagegroup.md)
