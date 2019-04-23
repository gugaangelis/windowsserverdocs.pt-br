---
title: Usando o comando novo CaptureImage
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2dfd08f0-be59-4715-96e6-c498305873f4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7d9f402acb9904624bdb4193a4306d57b104eda8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888607"
---
# <a name="using-the-new-captureimage-command"></a>Usando o comando novo CaptureImage



cria uma nova imagem de captura de uma imagem de inicialização existente. Imagens de captura são utilitário em vez de iniciar a instalação de captura de imagens de inicialização que iniciem os serviços de implantação do Windows. Quando você inicializar um computador de referência (o que foi preparado com Sysprep) em uma imagem de captura, um assistente cria uma imagem de instalação do computador de referência e o salva como um arquivo de imagem do Windows (. wim). Você pode também adicionar a imagem à mídia (como uma unidade de CD, DVD ou USB) e, em seguida, inicialize um computador a partir dessa mídia. Depois de criar a imagem de instalação, você pode adicionar a imagem para o servidor para implantação de inicialização PXE. Para obter mais informações, consulte Criando imagens ([https://go.microsoft.com/fwlink/?LinkId=115311](https://go.microsoft.com/fwlink/?LinkId=115311)).

## <a name="syntax"></a>Sintaxe

```
WDSUTIL [Options] /New-CaptureImage [/Server:<Server name>]
     /Image:<Image name>
     /Architecture:{x86 | ia64 | x64}
     [/Filename:<File name>]
     /DestinationImage
        /FilePath:<File path and name>
        [/Name:<Name>]
        [/Description:<Description>]
        [/Overwrite:{Yes | No | Append}]
        [/UnattendFilePath:<File path>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[/Server:\<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|/ Imagem:\<nome da imagem >|Especifica o nome da imagem de inicialização de origem.|
|/Architecture: {x86 | ia64 | x64}|Especifica a arquitetura da imagem a ser usado. Como você pode ter o mesmo nome de imagem para imagens de inicialização diferente em diferentes arquiteturas, especificar que isso garante que a imagem correta é usada.|
|[/Filename: \<Nome do arquivo >]|Se a imagem não pode ser identificada exclusivamente pelo nome, você deve usar essa opção para especificar o nome do arquivo.|
|/DestinationImage|Especifica as configurações para a imagem de destino. Você especifica as configurações usando as seguintes opções:</br>-   /FilePath: \<Caminho e nome do arquivo > define o caminho completo para a nova imagem de captura.</br>-[/Name: \<Nome >]-define o nome de exibição da imagem. Se nenhum nome de exibição for especificada, será usado o nome de exibição da imagem de origem.</br>-[/ Descrição: \<Descrição >]-define a descrição da imagem.</br>-[/Overwrite: {Sim | Não | Acrescentar}] - determina se o arquivo especificado na **/DestinationImage** deve ser substituído se outro arquivo com esse nome já existe no /FilePath. **Sim** substituirá o arquivo existente. **Não** (padrão) causa um erro ocorra se outro arquivo com o mesmo nome já existe. **Acrescentar** anexa a imagem gerada como uma nova imagem no arquivo. wim existente.</br>-   [/UnattendFilePath: \<Caminho do arquivo >]-define o caminho completo e nome para o arquivo de captura de imagem autônomo.|

## <a name="BKMK_examples"></a>Exemplos

Para criar uma imagem de captura e nomeie-o WinPECapture.wim, digite:
```
WDSUTIL /New-CaptureImage /Image:"WinPE boot image" /Architecture:x86 /DestinationImage /FilePath:"C:\Temp\WinPECapture.wim"

```
Para criar uma imagem de captura e aplicar as configurações especificadas, digite:
```
WDSUTIL /Verbose /Progress /New-CaptureImage /Server:MyWDSServer /Image:"WinPE boot image" /Architecture:x64 /Filename:boot.wim 
/DestinationImage /FilePath:"\\Server\Share\WinPECapture.wim" /Name:"New WinPE image" /Description:"WinPE image with capture utility" /Overwrite:No /UnattendFilePath:"\\Server\Share\WDSCapture.inf"
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)