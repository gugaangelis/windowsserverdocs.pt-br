---
title: Adicionar um MyImage
description: Tópico de comandos do Windows para Add-Image Group, que adiciona um grupo de imagens a um servidor de serviços de implantação do Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6ca88671-51de-4924-b969-88f3dfd84270
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f12ed27ca1a809ec34dbefbc4ff7288ff194a83e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831889"
---
# <a name="add-imagegroup"></a>Adicionar um MyImage

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Adiciona um grupo de imagens a um servidor dos serviços de implantação do Windows.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /add-ImageGroumediaGroup:<Image group name> [/Server:<Server name>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
<Image group name> de mídia:|Especifica o nome do grupo de imagens a ser adicionado.|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se um nome do servidor não for especificado, o servidor local será usado.|
## <a name="examples"></a><a name=BKMK_examples></a>Disso
Para adicionar um grupo de imagens, digite um dos seguintes:
```
wdsutil /add-ImageGroumediaGroup:ImageGroup2
wdsutil /verbose /add-ImageGroumediaGroup:My Image Group /Server:MyWDSServer
```
## <a name="additional-references"></a>Referências adicionais
- A [chave de sintaxe de linha de comando](command-line-syntax-key.md)
usando o [comando get-AllImageGroups](using-the-get-allimagegroups-command.md)
[usando o comando get-Image-](using-the-get-imagegroup-command.md) do
[usando o comando remove-Image](using-the-remove-imagegroup-command.md) do grupo de imagens
[subcomando: Set-MyImage](subcommand-set-imagegroup.md)
