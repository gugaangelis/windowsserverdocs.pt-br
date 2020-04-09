---
title: New-CaptureImage
description: O tópico de comandos do Windows para New-CaptureImage, que cria uma nova imagem de captura de uma imagem de inicialização existente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2dfd08f0-be59-4715-96e6-c498305873f4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9d8847888b87dfadb25cbb79dc172bf9b721b819
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830739"
---
# <a name="new-captureimage"></a>New-CaptureImage

Cria uma nova imagem de captura a partir de uma imagem de inicialização existente. Imagens de captura são imagens de inicialização que iniciam o utilitário de captura dos serviços de implantação do Windows em vez de iniciar a instalação. Quando você inicializa um computador de referência (que foi preparado com o Sysprep) em uma imagem de captura, um assistente cria uma imagem de instalação do computador de referência e salva-o como um arquivo de imagem do Windows (. wim). Você também pode adicionar a imagem à mídia (como um CD, DVD ou unidade USB) e, em seguida, inicializar um computador por meio dessa mídia. Depois de criar a imagem de instalação, você pode adicionar a imagem ao servidor para a implantação de inicialização PXE. Para obter mais informações, consulte Creating images ([https://go.microsoft.com/fwlink/?LinkId=115311](https://go.microsoft.com/fwlink/?LinkId=115311)).

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
| [/Server: nome do servidor\<>] |                                                                                                                                       Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.                                                                                                                                        |
|   /Image: nome da imagem de\<>   |                                                                                                                                                                                                         Especifica o nome da imagem de inicialização de origem.                                                                                                                                                                                                         |
|   /Architecture: {x86    |                                                                                                                                                                                                                             Win64                                                                                                                                                                                                                             |
| [/Filename: \<filename >] |                                                                                                                                                                            Se a imagem não puder ser identificada exclusivamente pelo nome, você deverá usar essa opção para especificar o nome do arquivo.                                                                                                                                                                            |
|    /DestinationImage     | Especifica as configurações para a imagem de destino. Você especifica as configurações usando as seguintes opções:</br>-/FilePath.: caminho e nome do arquivo de \<> define o caminho completo do arquivo para a nova imagem de captura.</br>-[/Name: \<Name >] – define o nome de exibição da imagem. Se nenhum nome de exibição for especificado, o nome de exibição da imagem de origem será usado.</br>-[/Description: \<Description >] – define a descrição da imagem.</br>-[/Overwrite: {Sim |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

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