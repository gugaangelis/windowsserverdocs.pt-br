---
title: New-CaptureImage
description: Artigo de referência para New-CaptureImage, que cria uma nova imagem de captura de uma imagem de inicialização existente.
ms.topic: reference
ms.assetid: 2dfd08f0-be59-4715-96e6-c498305873f4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5e98b0b52df39f20b9c84a55aa96c569943b556e
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038164"
---
# <a name="new-captureimage"></a>New-CaptureImage

Cria uma nova imagem de captura a partir de uma imagem de inicialização existente. Imagens de captura são imagens de inicialização que iniciam o utilitário de captura dos serviços de implantação do Windows em vez de iniciar a instalação. Quando você inicializa um computador de referência (que foi preparado com o Sysprep) em uma imagem de captura, um assistente cria uma imagem de instalação do computador de referência e salva-o como um arquivo de imagem do Windows (. wim). Você também pode adicionar a imagem à mídia (como um CD, DVD ou unidade USB) e, em seguida, inicializar um computador por meio dessa mídia. Depois de criar a imagem de instalação, você pode adicionar a imagem ao servidor para a implantação de inicialização PXE. Para obter mais informações, consulte Creating images ( [https://go.microsoft.com/fwlink/?LinkId=115311](https://go.microsoft.com/fwlink/?LinkId=115311) ).

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

### <a name="parameters"></a>Parâmetros

|        Parâmetro         |                                                                                                                                                                                                                         Descrição                                                                                                                                                                                                                          |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server:\<Server name>] |                                                                                                                                       Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.                                                                                                                                        |
|   /Image:\<Image name>   |                                                                                                                                                                                                         Especifica o nome da imagem de inicialização de origem.                                                                                                                                                                                                         |
|   /Architecture: {x86    |                                                                                                                                                                                                                             Win64                                                                                                                                                                                                                             |
| [/Filename: \<Filename> ] |                                                                                                                                                                            Se a imagem não puder ser identificada exclusivamente pelo nome, você deverá usar essa opção para especificar o nome do arquivo.                                                                                                                                                                            |
|    /DestinationImage     | Especifica as configurações para a imagem de destino. Você especifica as configurações usando as seguintes opções:</br>-/FilePath.: \<File path and name> define o caminho de arquivo completo para a nova imagem de captura.</br>-[/Name: \<Name> ] – define o nome de exibição da imagem. Se nenhum nome de exibição for especificado, o nome de exibição da imagem de origem será usado.</br>-[/Description: \<Description> ] – define a descrição da imagem.</br>-[/Overwrite: {Sim |

## <a name="examples"></a>Exemplos

Para criar uma imagem de captura e nomeá-la WinPECapture. wim, digite:
```
WDSUTIL /New-CaptureImage /Image:WinPE boot image /Architecture:x86 /DestinationImage /FilePath:C:\Temp\WinPECapture.wim
```
Para criar uma imagem de captura e aplicar as configurações especificadas, digite:
```
WDSUTIL /Verbose /Progress /New-CaptureImage /Server:MyWDSServer /Image:WinPE boot image /Architecture:x64 /Filename:boot.wim
/DestinationImage /FilePath:\\Server\Share\WinPECapture.wim /Name:New WinPE image /Description:WinPE image with capture utility /Overwrite:No /UnattendFilePath:\\Server\Share\WDSCapture.inf
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)