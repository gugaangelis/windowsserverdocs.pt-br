---
title: New-DiscoverImage
description: Tópico de referência para New-DiscoverImage, que cria uma nova imagem de descoberta de uma imagem de inicialização existente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ede9fbbb-0bba-4309-8c21-3cc13e1dc3cd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 45202f2bde3ca045a27e604743c25659df2436f9
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725790"
---
# <a name="new-discoverimage"></a>New-DiscoverImage

Cria uma nova imagem de descoberta de uma imagem de inicialização existente. As imagens de descoberta são imagens de inicialização que forçam o programa Setup. exe a iniciar no modo de serviços de implantação do Windows e, em seguida, descobrir um servidor dos serviços de implantação do Windows. Normalmente, essas imagens são usadas para implantar imagens em computadores que não são capazes de inicializar para PXE. Para obter mais informações, consulte Creating images ([https://go.microsoft.com/fwlink/?LinkId=115311](https://go.microsoft.com/fwlink/?LinkId=115311)).

## <a name="syntax"></a>Sintaxe

```
WDSUTIL [Options] /New-DiscoverImage [/Server:<Server name>]
     /Image:<Image name>
     /Architecture:{x86 | ia64 | x64}
     [/Filename:<File name>]
     /DestinationImage
         /FilePath:<File path and name>
         [/Name:<Name>]
         [/Description:<Description>]
         [/WDSServer:<Server name>]
         [/Overwrite:{Yes | No | Append}]
```

### <a name="parameters"></a>Parâmetros

|        Parâmetro         |                                                                                                                                                                                                                                                                                                                                                                                                                       Descrição                                                                                                                                                                                                                                                                                                                                                                                                                       |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server:\<nome do servidor>] |                                                                                                                                                                                                                                                                                                                                     Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.                                                                                                                                                                                                                                                                                                                                     |
|   /Image:\<nome da imagem>   |                                                                                                                                                                                                                                                                                                                                                                                                      Especifica o nome da imagem de inicialização de origem.                                                                                                                                                                                                                                                                                                                                                                                                       |
|    /Architecture: {x86    |                                                                                                                                                                                                                                                                                                                                                                                                                          Win64                                                                                                                                                                                                                                                                                                                                                                                                                           |
| [/Filename:\<nome do arquivo>] |                                                                                                                                                                                                                                                                                                                                                                         Se a imagem não puder ser identificada exclusivamente pelo nome, você deverá usar essa opção para especificar o nome do arquivo.                                                                                                                                                                                                                                                                                                                                                                          |
|    /DestinationImage     | Especifica as configurações para a imagem de destino. Você pode especificar as configurações usando as seguintes opções:</br>-/FilePath.: < caminho e nome do arquivo>-define o caminho completo do arquivo para a nova imagem.</br>-[/Name:\<Name>] – define o nome de exibição da imagem. Se nenhum nome de exibição for especificado, o nome de exibição da imagem de origem será usado.</br>-[/Description: \<Description>] – define a descrição da imagem.</br>-[/WDSServer: \<nome do servidor>] – Especifica o nome do servidor que todos os clientes que inicializam da imagem especificada devem contatar para baixar a imagem de instalação. Por padrão, todos os clientes que inicializam essa imagem descobrirão um servidor válido dos serviços de implantação do Windows. O uso dessa opção ignora a funcionalidade de descoberta e força o cliente inicializado a entrar em contato com o servidor especificado.</br>-[/Overwrite: {Sim |

## <a name="examples"></a>Exemplos

Para criar uma imagem de descoberta fora da imagem de inicialização e nomeá-la WinPEDiscover. wim, digite:
```
WDSUTIL /New-DiscoverImage /Image:WinPE boot image /Architecture:x86 /DestinationImage /FilePath:C:\Temp\WinPEDiscover.wim
```
Para criar uma imagem de descoberta fora da imagem de inicialização e nomeie-a WinPEDiscover. wim com as configurações especificadas, digite:
```
WDSUTIL /Verbose /Progress /New-DiscoverImage /Server:MyWDSServer
/Image:WinPE boot image /Architecture:x64 /Filename:boot.wim /DestinationImage /FilePath:\\Server\Share\WinPEDiscover.wim 
/Name:New WinPE image /Description:WinPE image for WDS Client discovery /Overwrite:No
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)