---
title: WDSUTIL Get-myImages
description: Artigo de referência para o WDSUTIL Get-myImages, que recupera informações sobre todas as imagens em um servidor.
ms.topic: reference
ms.assetid: 19de3720-4315-415a-8dc6-486caa0b2100
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: fc4234e199d0eb39c3f8d94c31f2330f60cdc167
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729662"
---
# <a name="wdsutil-get-allimages"></a>WDSUTIL Get-myImages

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
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [comando de adição de imagem do WDSUTIL](wdsutil-add-image.md)
- [comando de cópia de imagem do WDSUTIL](wdsutil-copy-image.md)
- [comando WDSUTIL Export-Image](wdsutil-export-image.md)
- [comando de remoção de imagem do WDSUTIL](wdsutil-remove-image.md)
- [comando de substituição do WDSUTIL-imagem](wdsutil-replace-image.md)
- [WDSUTIL Set-Command Image](wdsutil-set-image.md)
