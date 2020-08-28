---
title: Get-MulticastTransmission
description: Artigo de referência para Get-MulticastTransmission, que exibe informações sobre a transmissão multicast para uma imagem especificada.
ms.topic: reference
ms.assetid: b733737b-1e81-43d4-a058-d6985a613bef
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9c5ac51a3ae3d17f21e15dafb940df8c020b221e
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89029524"
---
# <a name="get-multicasttransmission"></a>Get-MulticastTransmission

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
MediaType: instalar|Especifica o tipo de imagem. Observe que essa opção deve ser definida como **instalar**.|
|\mediaGroup: <Image group name> ]|Especifica o grupo de imagens que contém a imagem. Se nenhum nome de grupo de imagens for especificado e houver apenas um grupo de imagens no servidor, esse grupo de imagens será usado. Se houver mais de um grupo de imagens no servidor, você deverá usar essa opção para especificar um grupo de imagens.|
|/Architecture: {x86 &#124; IA64 &#124; x64}|Especifica a arquitetura da imagem de inicialização associada à transmissão. Como é possível ter o mesmo nome de imagem para imagens de inicialização em diferentes arquiteturas, você deve especificar a arquitetura para garantir que a imagem correta seja usada.|
|[/Filename:<File name>]|Especifica o arquivo que contém a imagem. Se a imagem não puder ser identificada exclusivamente pelo nome, você deverá usar essa opção para especificar o nome do arquivo.|
|[/Show: clients]<p>ou<p>[/details: clientes]|Exibe informações sobre os computadores cliente que estão conectados à transmissão multicast.|
## <a name="examples"></a>Exemplos
**Windows Server 2008** Para exibir informações sobre a transmissão de uma imagem chamada vista com o Office, digite um dos seguintes:
```
wdsutil /Get-MulticastTransmissiomedia:Vista with Officemediatype:Install
wdsutil /Get-MulticastTransmission /Server:MyWDSServemedia:Vista with Officemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim /Show:Clients
```
**Windows Server 2008 R2** Para exibir informações sobre a transmissão de uma imagem chamada vista com o Office, digite um dos seguintes:
```
wdsutil /Get-MulticastTransmissiomedia:Vista with Office
 /Imagetype:Install
```
```
wdsutil /Get-MulticastTransmission /Server:MyWDSServemedia:Vista with Officemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim /details:Clients
```
```
wdsutil /Get-MulticastTransmission /Server:MyWDSServemedia:X64 Boot Imagemediatype:Boot /Architecture:x64 /Filename:boot.wim /details:Clients
```
## <a name="additional-references"></a>Referências adicionais
- Chave de sintaxe [de linha de comando](command-line-syntax-key.md) 
 [Usando o comando](using-the-get-allmulticasttransmissions-command.md) 
 Get-AllMulticastTransmissions [Usando o comando](using-the-new-multicasttransmission-command.md) 
 New-MulticastTransmission [Usando o comando](using-the-remove-multicasttransmission-command.md) 
 Remove-MulticastTransmission [Subcomando: Start-MulticastTransmission](subcommand-start-multicasttransmission.md)
