---
title: Get-myImages
description: Tópico de referência para as imagens Get-My, que recupera informações sobre todas as imagens em um servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 19de3720-4315-415a-8dc6-486caa0b2100
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c1f32a1789b22d04b7b61979d0ea49d91f0cf157
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720025"
---
# <a name="get-allimages"></a>Get-myImages

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera informações sobre todas as imagens em um servidor.

## <a name="syntax"></a>Sintaxe
```
wdsutil /Get-AllImages [/Server:<Server name>] /Show:{Boot | Install | LegacyRis | All} [/detailed]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|/Show: {boot &#124; install &#124; LegacyRis &#124; todos}|-   A **inicialização** retorna apenas imagens de inicialização.<br />-   **Instalar** retorna imagens de instalação, bem como informações sobre os grupos de imagens que os contêm.<br />-   **LegacyRis** retorna somente imagens de RIS (serviços de instalação remota).<br />-   **All** retorna informações de imagem de inicialização, informações de imagem de instalação (incluindo informações sobre os grupos de imagens) e informações de imagem do RIS.|
|[/detailed]|Indica que todos os metadados de imagem de cada imagem devem ser retornados. Se essa opção não for usada, o comportamento padrão será retornar apenas o nome da imagem, a descrição e o nome do arquivo.|
## <a name="examples"></a>Exemplos
Para exibir informações sobre as imagens, digite um dos seguintes:
```
wdsutil /Get-AllImages /Show:Install
wdsutil /verbose /Get-AllImages /Server:MyWDSServer /Show:All /detailed
```
## <a name="additional-references"></a>Referências adicionais
- [Chave](command-line-syntax-key.md)
de sintaxe de linha de comando usando o
[comando](using-the-add-image-command.md)
Add-Image
[usando o comando copy-Image](using-the-copy-image-command.md)
[usando o comando Export-Image](using-the-export-image-command.md)
[usando o comando Remove-](using-the-remove-image-command.md)Image[usando o subcomando Replace-Image Command](using-the-replace-image-command.md)[: Set-Image](subcommand-set-image.md)
