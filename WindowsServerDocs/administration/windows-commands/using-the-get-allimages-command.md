---
title: Usando o comando Get-myImages
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 19de3720-4315-415a-8dc6-486caa0b2100
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5122a5660031d503795715c0005b404f910d6626
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363502"
---
# <a name="using-the-get-allimages-command"></a>Usando o comando Get-myImages

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera informações sobre todas as imagens em um servidor.
## <a name="syntax"></a>Sintaxe
```
wdsutil /Get-AllImages [/Server:<Server name>] /Show:{Boot | Install | LegacyRis | All} [/detailed]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|/Show: {boot &#124; install &#124; LegacyRis &#124; All}|a**inicialização** -    retorna apenas imagens de inicialização.<br />a**instalação** do -    retorna imagens de instalação, bem como informações sobre os grupos de imagens que os contêm.<br />-   **LegacyRis** retorna somente as imagens de RIS (serviços de instalação remota).<br />-   **All** retorna informações de imagem de inicialização, informações de imagem de instalação (incluindo informações sobre os grupos de imagens) e informações de imagem do RIS.|
|[/detailed]|Indica que todos os metadados de imagem de cada imagem devem ser retornados. Se essa opção não for usada, o comportamento padrão será retornar apenas o nome da imagem, a descrição e o nome do arquivo.|
## <a name="BKMK_examples"></a>Disso
Para exibir informações sobre as imagens, digite um dos seguintes:
```
wdsutil /Get-AllImages /Show:Install
wdsutil /verbose /Get-AllImages /Server:MyWDSServer /Show:All /detailed
```
#### <a name="additional-references"></a>Referências adicionais
A [chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando add-Image](using-the-add-image-command.md)
[usando o comando copy-Image](using-the-copy-image-command.md)
[usando o comando Export-Image](using-the-export-image-command.md)
[usando o comando Remove-Image](using-the-remove-image-command.md)
[usando o substituir o comando de imagem](using-the-replace-image-command.md)1[subcomando: Set-Image](subcommand-set-image.md)
