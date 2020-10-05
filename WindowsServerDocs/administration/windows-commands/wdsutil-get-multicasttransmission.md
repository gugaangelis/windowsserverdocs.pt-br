---
title: WDSUTIL Get-MulticastTransmission
description: Artigo de referência para WDSUTIL Get-MulticastTransmission, que exibe informações sobre a transmissão multicast para uma imagem especificada.
ms.topic: reference
ms.assetid: b733737b-1e81-43d4-a058-d6985a613bef
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ceb31ea1d1d9a266c7b8ab13a859275140b98317
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729638"
---
# <a name="wdsutil-get-multicasttransmission"></a>WDSUTIL Get-MulticastTransmission

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe informações sobre a transmissão multicast para uma imagem especificada.

## <a name="syntax"></a>Syntax
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
para transmissões de imagem de instalação:
```
wdsutil [Options] /Get-MulticastTransmissiomedia:<Image name>
         [/Server:<Server name>]
         [/details:Clients]
       mediatype:Install
    mediaGroup:<Image Group>]
     [/Filename:<File name>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
meio<Image name>|Exibe a transmissão multicast associada a esta imagem.|
|[/Server:<Server name>]|Especifica o nome do servidor. Esse pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|/ImageType: instalar|Especifica o tipo de imagem. Observe que essa opção deve ser definida como **instalar**.|
|/ImageGroup: <Image group name> ]|Especifica o grupo de imagens que contém a imagem. Se nenhum nome de grupo de imagens for especificado e houver apenas um grupo de imagens no servidor, esse grupo de imagens será usado. Se houver mais de um grupo de imagens no servidor, você deverá usar essa opção para especificar um grupo de imagens.|
|/Architecture: {x86 &#124; IA64 &#124; x64}|Especifica a arquitetura da imagem de inicialização associada à transmissão. Como é possível ter o mesmo nome de imagem para imagens de inicialização em diferentes arquiteturas, você deve especificar a arquitetura para garantir que a imagem correta seja usada.|
|[/Filename:<File name>]|Especifica o arquivo que contém a imagem. Se a imagem não puder ser identificada exclusivamente pelo nome, você deverá usar essa opção para especificar o nome do arquivo.|
|[/Show: clients]<p>ou<p>[/details: clientes]|Exibe informações sobre os computadores cliente que estão conectados à transmissão multicast.|
## <a name="examples"></a>Exemplos
**Windows Server 2008** Para exibir informações sobre a transmissão de uma imagem chamada vista com o Office, digite um dos seguintes:
```
wdsutil /Get-MulticastTransmission:Vista with Office imagetype:Install
wdsutil /Get-MulticastTransmission /Server:MyWDSServer image:Vista with Office imagetype:Install imageGroup:ImageGroup1 /Filename:install.wim /Show:Clients
```
**Windows Server 2008 R2** Para exibir informações sobre a transmissão de uma imagem chamada vista com o Office, digite um dos seguintes:
```
wdsutil /Get-MulticastTransmission:Vista with Office
 /Imagetype:Install
```
```
wdsutil /Get-MulticastTransmission /Server:MyWDSServer image:Vista with Office imagetype:Install ImageGroup:ImageGroup1 /Filename:install.wim /details:Clients
```
```
wdsutil /Get-MulticastTransmission /Server:MyWDSServer:X64 Boot Imagetype:Boot /Architecture:x64 /Filename:boot.wim /details:Clients
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [comando WDSUTIL Get-allmulticasttransmissions](wdsutil-get-allmulticasttransmissions.md)
- [comando WDSUTIL New-MulticastTransmission](wdsutil-new-multicasttransmission.md)
- [comando WDSUTIL Remove-MulticastTransmission](wdsutil-remove-multicasttransmission.md)
- [comando WDSUTIL Start-MulticastTransmission](wdsutil-start-multicasttransmission.md)
