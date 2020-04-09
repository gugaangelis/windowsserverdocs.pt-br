---
title: Get-MyImage
description: O tópico comandos do Windows para Get-Image Group, que recupera informações sobre um grupo de imagens e as imagens contidas nele.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0fc25aca-a529-44ee-bc8e-96bc8affb458
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0066e5d52c1d10b1f78ea627ee7a476bfd98f19d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830949"
---
# <a name="get-imagegroup"></a>Get-MyImage

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera informações sobre um grupo de imagens e as imagens dentro dele.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Get-ImageGroumediaGroup:<Image group name> [/Server:<Server name>] [/detailed]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
<Image group name> de mídia:|Especifica o nome do grupo de imagens.|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|[/detailed]|Retorna os metadados da imagem para cada imagem. Se esse parâmetro não for usado, o comportamento padrão será retornar apenas o nome da imagem, a descrição e o nome do arquivo.|
## <a name="examples"></a><a name=BKMK_examples></a>Disso
Para exibir informações sobre um grupo de imagens, digite:
```
wdsutil /Get-ImageGroumediaGroup:ImageGroup1
```
Para exibir informações, incluindo metadados, digite:
```
wdsutil /verbose /Get-ImageGroumediaGroup:ImageGroup1 /Server:MyWDSServer /detailed
```
## <a name="additional-references"></a>Referências adicionais
- [A chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando Add-Image](using-the-add-imagegroup-command.md) do
[usando o comando Get-AllImageGroups](using-the-get-allimagegroups-command.md)
[usando o comando remove-Image](using-the-remove-imagegroup-command.md) do grupo de imagens
[subcomando: Set-MyImage](subcommand-set-imagegroup.md)
