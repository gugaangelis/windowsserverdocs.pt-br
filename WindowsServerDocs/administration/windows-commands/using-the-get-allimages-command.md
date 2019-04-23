---
title: Usando o comando get-AllImages
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 57b81dd3dd3a24876c4401e80d08130ed5243888
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872557"
---
# <a name="using-the-get-allimages-command"></a>Usando o comando get-AllImages

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
|/ Mostrar: {inicialização &#124; instalar &#124; LegacyRis &#124; todos os}|-   **Inicialização** retorna apenas imagens de inicialização.<br />-   **Instalar** retorna instalará imagens, bem como informações sobre os grupos de imagens que os contêm.<br />-   **LegacyRis** retorna apenas imagens de instalação RIS (serviços) remoto.<br />-   **Todos os** retorna informações de imagem do RIS, informações de imagem de instalação (incluindo informações sobre os grupos de imagem) e informações da imagem de inicialização.|
|[/ detalhadas]|Indica que todos os metadados de imagem de cada imagem devem ser retornado. Se essa opção não for usada, o comportamento padrão é retornar somente o nome da imagem, descrição e nome de arquivo.|
## <a name="BKMK_examples"></a>Exemplos
Para exibir informações sobre as imagens, digite o seguinte:
```
wdsutil /Get-AllImages /Show:Install
wdsutil /verbose /Get-AllImages /Server:MyWDSServer /Show:All /detailed
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando add-Image](using-the-add-image-command.md)
[usando a comando de copiar imagem](using-the-copy-image-command.md)
[usando o Comando de Export-Image](using-the-export-image-command.md)
[usando o comando de remove-Image](using-the-remove-image-command.md)
[usando a comando de substituir imagem](using-the-replace-image-command.md) 
 [Subcomando: Definir imagem](subcommand-set-image.md)
