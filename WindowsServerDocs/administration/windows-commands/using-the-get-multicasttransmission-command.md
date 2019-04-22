---
title: Usando o comando get-MulticastTransmission
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b733737b-1e81-43d4-a058-d6985a613bef
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fdbb9283285bcf56cd83c18ea076e3d36a51b966
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823227"
---
# <a name="using-the-get-multicasttransmission-command"></a>Usando o comando get-MulticastTransmission

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe informações sobre a transmissão multicast para uma imagem especificada.
## <a name="syntax"></a>Sintaxe
**Windows Server 2008**
```
wdsutil [Options] /Get-MulticastTransmissiomedia:<Image name> [/Server:<Server name>mediatype:InstallmediaGroup:<Image group name>] 
[/Filename:<File name>] [/Show:Clients]
```
**Windows Server 2008 R2** para transmissões de imagem de inicialização:
```
wdsutil [Options] /Get-MulticastTransmissiomedia:<Image name>
    [/Server:<Server name>]
    [/details:Clients]
  mediatype:Boot
    /Architecture:{x86 | ia64 | x64}
        [/Filename:<File name>]
```
para as transmissões de imagem de instalação:
```
wdsutil [Options] /Get-MulticastTransmissiomedia:<Image name>
         [/Server:<Server name>]
         [/details:Clients]
       mediatype:Install
    mediaGroup:<Image Group>]
     [/Filename:<File name>]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
mídia:<Image name>|Exibe a transmissão multicast que está associada esta imagem.|
|[/Server:<Server name>]|Especifica o nome do servidor. Isso pode ser o nome NetBIOS ou o nome de domínio totalmente qualificado (FQDN). Se nenhum nome de servidor for especificado, o servidor local será usado.|
mediatype:Install|Especifica o tipo de imagem. Observe que essa opção deve ser definida como **instalar**.|
|\mediaGroup:<Image group name>]|Especifica o grupo de imagens que contém a imagem. Se nenhum nome de grupo de imagem for especificado e o grupo de apenas uma imagem existe no servidor, esse grupo é usado. Se existir mais de um grupo de imagens no servidor, você deve usar essa opção para especificar um grupo de imagens.|
|/ Arquitetura: {x86 &#124; ia64 &#124; x64}|Especifica a arquitetura da imagem de inicialização que está associada com a transmissão. Como é possível ter o mesmo nome de imagem para imagens de inicialização em arquiteturas diferentes, você deve especificar a arquitetura para garantir que a imagem correta é usada.|
|[/Filename:<File name>]|Especifica o arquivo que contém a imagem. Se a imagem não pode ser identificada exclusivamente pelo nome, você deve usar essa opção para especificar o nome do arquivo.|
|[/Show:Clients]<br /><br />ou<br /><br />[/details:Clients]|Exibe informações sobre computadores cliente que estão conectados a transmissão multicast.|
## <a name="BKMK_examples"></a>Exemplos
**Windows Server 2008** para exibir informações sobre a transmissão para uma imagem chamada Vista com o Office, digite o seguinte:
```
wdsutil /Get-MulticastTransmissiomedia:"Vista with Officemediatype:Install
wdsutil /Get-MulticastTransmission /Server:MyWDSServemedia:"Vista with Officemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim /Show:Clients
```
**Windows Server 2008 R2** para exibir informações sobre a transmissão para uma imagem chamada Vista com o Office, digite o seguinte:
```
wdsutil /Get-MulticastTransmissiomedia:"Vista with Office"
 /Imagetype:Install
```
```
wdsutil /Get-MulticastTransmission /Server:MyWDSServemedia:"Vista with Officemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim /details:Clients
```
```
wdsutil /Get-MulticastTransmission /Server:MyWDSServemedia:"X64 Boot Imagemediatype:Boot /Architecture:x64 /Filename:boot.wim /details:Clients
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando get-AllMulticastTransmissions](using-the-get-allmulticasttransmissions-command.md)
[usando o comando /New-MulticastTransmission](using-the-new-multicasttransmission-command.md) 
 [Usando o comando remove-MulticastTransmission](using-the-remove-multicasttransmission-command.md)
[subcomando: início MulticastTransmission](subcommand-start-multicasttransmission.md)
