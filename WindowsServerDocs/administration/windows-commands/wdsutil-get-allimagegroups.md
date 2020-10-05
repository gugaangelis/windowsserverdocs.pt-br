---
title: WDSUTIL Get-allimagegroups
description: Artigo de referência para o WDSUTIL Get-allimagegroups, que recupera informações sobre todos os grupos de imagens em um servidor e todas as imagens nesses grupos de imagens.
ms.topic: reference
ms.assetid: 2ca06533-bcf5-4590-ac8e-263d6c9874f8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 894c9ede958b4583c03d4b0e3b5ac4cca1f39395
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729665"
---
# <a name="wdsutil-get-allimagegroups"></a>WDSUTIL Get-allimagegroups

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera informações sobre todos os grupos de imagens em um servidor e todas as imagens nesses grupos de imagens.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Get-AllImageGroups [/Server:<Server name>] [/detailed]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|[/detailed]|Retorna os metadados da imagem de cada imagem. Se esse parâmetro não for usado, o comportamento padrão será retornar apenas o nome da imagem, a descrição e o nome do arquivo para cada imagem.|
## <a name="examples"></a>Exemplos
Para exibir informações sobre os grupos de imagens, digite um dos seguintes:
```
wdsutil /Get-AllImageGroups
wdsutil /verbose /Get-AllImageGroups /Server:MyWDSServer /detailed
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [comando de Add-Image do WDSUTIL](wdsutil-add-imagegroup.md)
- [comando WDSUTIL Get-Imageobject](wdsutil-get-imagegroup.md)
- [comando de remoção do WDSUTIL-MyImage](wdsutil-remove-imagegroup.md)
- [conjunto WDSUTIL-comando do grupo de imagens](wdsutil-set-imagegroup.md)
