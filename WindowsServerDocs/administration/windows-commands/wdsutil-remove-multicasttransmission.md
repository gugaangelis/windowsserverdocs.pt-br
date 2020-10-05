---
title: WDSUTIL Remove-MulticastTransmission
description: Artigo de referência para WDSUTIL Remove-MulticastTransmission, que desabilita a transmissão multicast para uma imagem.
ms.topic: reference
ms.assetid: 9a7f5c31-bfbf-425d-9129-a6f9173fe83d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a7ed8d9ef26deb0a50baa8511cd07f250561d579
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729842"
---
# <a name="wdsutil-remove-multicasttransmission"></a>WDSUTIL Remove-MulticastTransmission

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Desabilita a transmissão multicast para uma imagem. A menos que você especifique **/Force**, os clientes existentes concluirão a transferência de imagem, mas novos clientes não terão permissão para ingressar.

## <a name="syntax"></a>Syntax
**Windows Server 2008**
```
wdsutil /remove-MulticastTransmission:<Image name> [/Server:<Server name> mediatype:Install Group:<Image Group>] [/Filename:<File name>] [/force]
```
**Windows Server 2008 R2** para imagens de inicialização:
```
wdsutil [Options] /remove-MulticastTransmissiomedia:<Image name>
\x20    [/Server:<Server name>]
\x20  mediatype:Boot
\x20    /Architecture:{x86 | ia64 | x64}
\x20    [/Filename:<File name>]
```
para imagens de instalação:
```
wdsutil [Options] /remove-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
      mediatype:Install
       mediaGroup:<Image Group
        [/Filename:<File name>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|meio<Image name>|Especifica o nome da imagem.|
|[/Server:<Server name>]|Especifica o nome do servidor. Esse pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
MediaType: {instalar&#124;inicialização}|Especifica o tipo de imagem. Observe que essa opção deve ser definida como **instalar** para o Windows Server 2008.|
|/Architecture: {x86 &#124; IA64 &#124; x64}|Especifica a arquitetura da imagem de inicialização associada à transmissão a ser iniciada. Como é possível ter o mesmo nome de imagem para imagens de inicialização em diferentes arquiteturas, você deve especificar a arquitetura para garantir que a transmissão correta seja usada.|
|\mediaGroup: <Image group name> ]|Especifica o grupo de imagens que contém a imagem. Se nenhum nome de grupo de imagens for especificado e houver apenas um grupo de imagens no servidor, esse grupo de imagens será usado. Se houver mais de um grupo de imagens no servidor, você deverá usar essa opção para especificar o nome do grupo de imagens.|
|[/Filename:<File name>]|especifica o nome do arquivo. Se a imagem de origem não puder ser identificada exclusivamente pelo nome, você deverá usar essa opção para especificar o nome do arquivo.|
|/Force|Remove a transmissão e encerra todos os clientes. A menos que você especifique um valor para a opção **/Force** , os clientes existentes podem concluir a transferência de imagem, mas novos clientes não podem ingressar.|
## <a name="examples"></a>Exemplos
Para interromper um namespace (os clientes atuais concluirão a transmissão, mas novos clientes não poderão ingressar), digite:
```
wdsutil /remove-MulticastTransmission:Vista with Office
/Imagetype:Install
```
```
wdsutil /remove-MulticastTransmission:x64 Boot Image
/Imagetype:Boot /Architecture:x64
```
Para forçar o encerramento de todos os clientes, digite:
```
wdsutil /remove-MulticastTransmission /Server:MyWDSServer
/Image:Vista with Officemediatype:InstalmediaGroup:ImageGroup1
/Filename:install.wim /force
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [comando WDSUTIL Get-allmulticasttransmissions](wdsutil-get-allmulticasttransmissions.md)
- [comando WDSUTIL Get-MulticastTransmission](wdsutil-get-multicasttransmission.md)
- [comando WDSUTIL New-MulticastTransmission](wdsutil-new-multicasttransmission.md)
- [comando WDSUTIL Start-MulticastTransmission](wdsutil-start-multicasttransmission.md)
