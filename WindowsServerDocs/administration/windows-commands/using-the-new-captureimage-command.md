---
title: Usando o comando New-CaptureImage
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: fb48cb76ef99eac51b862a5e1a3d1999a1cfc89d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363052"
---
# <a name="using-the-new-captureimage-command"></a>Usando o comando New-CaptureImage



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

## <a name="parameters"></a>Parâmetros

|        Parâmetro         |                                                                                                                                                                                                                         Descrição                                                                                                                                                                                                                          |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server: \<Server nome >] |                                                                                                                                       Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.                                                                                                                                        |
|   /Image: nome de \<Image >   |                                                                                                                                                                                                         Especifica o nome da imagem de inicialização de origem.                                                                                                                                                                                                         |
|   /Architecture: {x86    |                                                                                                                                                                                                                             Win64                                                                                                                                                                                                                             |
| /Filename \<Filename >] |                                                                                                                                                                            se a imagem não puder ser identificada exclusivamente pelo nome, você deverá usar essa opção para especificar o nome do arquivo.                                                                                                                                                                            |
|    /DestinationImage     | Especifica as configurações para a imagem de destino. Você especifica as configurações usando as seguintes opções:</br>/FilePath. \<File e nome > define o caminho completo do arquivo para a nova imagem de captura.</br>-[/Name: \<Name >] – define o nome de exibição da imagem. Se nenhum nome de exibição for especificado, o nome de exibição da imagem de origem será usado.</br>- [/Description: \<Description >] – define a descrição da imagem.</br>-[/Overwrite: {Sim |

## <a name="BKMK_examples"></a>Disso

Para criar uma imagem de captura e nomeá-la WinPECapture. wim, digite:
```
WDSUTIL /New-CaptureImage /Image:"WinPE boot image" /Architecture:x86 /DestinationImage /FilePath:"C:\Temp\WinPECapture.wim"
```
Para criar uma imagem de captura e aplicar as configurações especificadas, digite:
```
WDSUTIL /Verbose /Progress /New-CaptureImage /Server:MyWDSServer /Image:"WinPE boot image" /Architecture:x64 /Filename:boot.wim 
/DestinationImage /FilePath:"\\Server\Share\WinPECapture.wim" /Name:"New WinPE image" /Description:"WinPE image with capture utility" /Overwrite:No /UnattendFilePath:"\\Server\Share\WDSCapture.inf"
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)