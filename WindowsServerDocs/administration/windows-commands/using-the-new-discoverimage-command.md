---
title: Usando o comando novo DiscoverImage
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ede9fbbb-0bba-4309-8c21-3cc13e1dc3cd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1212571fc07b912ab9fa3344b973ce5a25085d86
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835767"
---
# <a name="using-the-new-discoverimage-command"></a>Usando o comando novo DiscoverImage



Cria uma nova imagem de descoberta de uma imagem de inicialização existente. Descubra as imagens são imagens de inicialização que forçar o programa Setup.exe para iniciar no modo de serviços de implantação do Windows e, em seguida, descobrir um servidor de serviços de implantação do Windows. Normalmente, essas imagens são usadas para implantar imagens em computadores que não são capazes de inicializar para PXE. Para obter mais informações, consulte Criando imagens ([https://go.microsoft.com/fwlink/?LinkId=115311](https://go.microsoft.com/fwlink/?LinkId=115311)).

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

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[/Server:\<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|/ Imagem:\<nome da imagem >|Especifica o nome da imagem de inicialização de origem.|
|/Architecture:{x86 | ia64 | x64}|Especifica a arquitetura da imagem a ser retornado. Como é possível ter o mesmo nome de imagem para imagens de inicialização diferente em diferentes arquiteturas, especificando o valor de arquitetura garante que o WDSUTIL retorna a imagem correta.|
|[/ Nome de arquivo:\<nome do arquivo >]|Se a imagem não pode ser identificada exclusivamente pelo nome, você deve usar essa opção para especificar o nome do arquivo.|
|/DestinationImage|Especifica as configurações para a imagem de destino. Você pode especificar as configurações usando as seguintes opções:</br>-/FilePath: < caminho e nome do arquivo > - caminho de arquivo completo de conjuntos para a nova imagem.</br>-[/Name:\<nome >]-define o nome de exibição da imagem. Se nenhum nome de exibição for especificada, será usado o nome de exibição da imagem de origem.</br>-[/ Descrição: \<Descrição >]-define a descrição da imagem.</br>-[/ WDSServer: \<Nome do servidor >]-Especifica o nome do servidor que todos os clientes que inicializar a partir da imagem especificada devem contatar para baixar a imagem de instalação. Por padrão, todos os clientes que esta imagem de inicialização serão descobrir um servidor de serviços de implantação do Windows válido. Usando essa opção ignora a funcionalidade de descoberta e força o cliente inicializado, entre em contato com o servidor especificado.</br>-[/Overwrite: {Sim | Não | Acrescentar}] - determina se o arquivo especificado na **/DestinationImage** deve ser substituído se outro arquivo com esse nome já existe no /FilePath. **Sim** substituirá o arquivo existente. **Não** (padrão) causa um erro ocorra se outro arquivo com o mesmo nome já existe. **Acrescentar** anexa a imagem gerada como uma nova imagem no arquivo. wim existente.|

## <a name="BKMK_examples"></a>Exemplos

Para criar uma imagem de descoberta fora de imagem de inicialização e nomeie-o WinPEDiscover.wim, digite:
```
WDSUTIL /New-DiscoverImage /Image:"WinPE boot image" /Architecture:x86 /DestinationImage /FilePath:"C:\Temp\WinPEDiscover.wim"
```
Para criar uma imagem de descoberta fora de imagem de inicialização e nomeie-o WinPEDiscover.wim com as configurações especificadas, digite:
```
WDSUTIL /Verbose /Progress /New-DiscoverImage /Server:MyWDSServer
/Image:"WinPE boot image" /Architecture:x64 /Filename:boot.wim /DestinationImage /FilePath:"\\Server\Share\WinPEDiscover.wim" 
/Name:"New WinPE image" /Description:"WinPE image for WDS Client discovery" /Overwrite:No
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)