---
title: Usando o comando get-ImageGroup
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0fc25aca-a529-44ee-bc8e-96bc8affb458
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9dcb76155dc1044730673ed46a53cad57441a246
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862597"
---
# <a name="using-the-get-imagegroup-command"></a>Usando o comando get-ImageGroup

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera informações sobre um grupo de imagens e as imagens dentro dele.
## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Get-ImageGroumediaGroup:<Image group name> [/Server:<Server name>] [/detailed]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
mediaGroup:<Image group name>|Especifica o nome do grupo de imagens.|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|[/ detalhadas]|Retorna os metadados de imagem para cada imagem. Se esse parâmetro não for usado, o comportamento padrão é retornar somente o nome da imagem, descrição e nome de arquivo.|
## <a name="BKMK_examples"></a>Exemplos
Para exibir informações sobre um grupo de imagens, digite:
```
wdsutil /Get-ImageGroumediaGroup:ImageGroup1
```
Para exibir informações, incluindo metadados, digite:
```
wdsutil /verbose /Get-ImageGroumediaGroup:ImageGroup1 /Server:MyWDSServer /detailed
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando add-ImageGroup](using-the-add-imagegroup-command.md)
[usando o comando get-AllImageGroups](using-the-get-allimagegroups-command.md) 
 [ Usando o comando remove-ImageGroup](using-the-remove-imagegroup-command.md)
[subcomando: set-ImageGroup](subcommand-set-imagegroup.md)
