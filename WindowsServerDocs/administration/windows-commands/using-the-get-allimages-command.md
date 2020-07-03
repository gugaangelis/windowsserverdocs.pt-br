---
title: Get-myImages
description: Artigo de referência para as imagens Get-My, que recupera informações sobre todas as imagens em um servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 19de3720-4315-415a-8dc6-486caa0b2100
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9834552ebe6395f13333e81fbc2996a8ff49f39c
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85935168"
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
- Chave de sintaxe [de linha de comando](command-line-syntax-key.md) 
 [Usando o comando](using-the-add-image-command.md) 
 Add-Image [Usando o comando](using-the-copy-image-command.md) 
 Copy-Image [Usando o comando](using-the-export-image-command.md) 
 Export-Image [Usando o comando](using-the-remove-image-command.md) 
 Remove-Image [Usando o comando](using-the-replace-image-command.md) 
 replace-Image [Subcomando: Set-Image](subcommand-set-image.md)
