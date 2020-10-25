---
title: WDSUTIL Add-MyImage
description: Artigo de referência do comando WDSUTIL Add-Image Group, que adiciona um grupo de imagens a um servidor de serviços de implantação do Windows.
ms.topic: reference
ms.assetid: 6ca88671-51de-4924-b969-88f3dfd84270
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 3bffb562ba019bb55c783541b78c906dd4a08dc7
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524461"
---
# <a name="wdsutil-add-imagegroup"></a>WDSUTIL Add-MyImage

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Adiciona um grupo de imagens a um servidor dos serviços de implantação do Windows.

## <a name="syntax"></a>Sintaxe

```
wdsutil [Options] /add-ImageGroup imageGroup:<Imagegroupname> [/Server:<Server name>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| MyImage: `<Imagegroupname>` ] | Especifica o nome da imagem a ser adicionada. |
| [/Server:`<Servername>`] | Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se um nome de servidor não for especificado, o servidor local será usado. |

## <a name="examples"></a>Exemplos

Para adicionar um grupo de imagens, digite:

```
wdsutil /add-ImageGroup imageGroup:ImageGroup2
```

```
wdsutil /verbose /add-Imagegroup imageGroup:My Image Group /Server:MyWDSServer
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando WDSUTIL Get-allimagegroups](wdsutil-get-allimagegroups.md)

- [comando WDSUTIL Get-Imageobject](wdsutil-get-imagegroup.md)

- [comando de remoção do WDSUTIL-MyImage](wdsutil-remove-imagegroup.md)

- [conjunto WDSUTIL-comando do grupo de imagens](wdsutil-set-imagegroup.md)

- [Cmdlets dos serviços de implantação do Windows](/powershell/module/wds)
