---
title: WDSUTIL Get-MyImage
description: Artigo de referência para o WDSUTIL Get-Image Group, que recupera informações sobre um grupo de imagens e as imagens contidas nele.
ms.topic: reference
ms.assetid: 0fc25aca-a529-44ee-bc8e-96bc8affb458
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d40fef643ebbb8fd48f36fc048ddefc0b4b3229f
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729646"
---
# <a name="wdsutil-get-imagegroup"></a>WDSUTIL Get-MyImage

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera informações sobre um grupo de imagens e as imagens dentro dele.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Get-ImageGroup ImageGroup:<Image group name> [/Server:<Server name>] [/detailed]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/ImageGroup:<Image group name>|Especifica o nome do grupo de imagens.|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|[/detailed]|Retorna os metadados da imagem para cada imagem. Se esse parâmetro não for usado, o comportamento padrão será retornar apenas o nome da imagem, a descrição e o nome do arquivo.|
## <a name="examples"></a>Exemplos
Para exibir informações sobre um grupo de imagens, digite:
```
wdsutil /Get-ImageGroup ImageGroup:ImageGroup1
```
Para exibir informações, incluindo metadados, digite:
```
wdsutil /verbose /Get-ImageGroup ImageGroup:ImageGroup1 /Server:MyWDSServer /detailed
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [comando de Add-Image do WDSUTIL](wdsutil-add-imagegroup.md)
- [comando WDSUTIL Get-allimagegroups](wdsutil-get-allimagegroups.md)
- [comando de remoção do WDSUTIL-MyImage](wdsutil-remove-imagegroup.md)
- [conjunto WDSUTIL-comando do grupo de imagens](wdsutil-set-imagegroup.md)
