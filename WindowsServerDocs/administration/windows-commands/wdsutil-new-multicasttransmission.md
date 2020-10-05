---
title: WDSUTIL New-MulticastTransmission
description: Artigo de referência para WDSUTIL New-MulticastTransmission, que cria uma nova transmissão multicast para uma imagem.
ms.topic: reference
ms.assetid: c1f1dc46-dd50-4eb9-9f72-cf0e5d71bd3d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4b02668a6e714ce090a013a34d9dee73121f7a89
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729619"
---
# <a name="wdsutil-new-multicasttransmission"></a>WDSUTIL New-MulticastTransmission

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria uma nova transmissão multicast para uma imagem. Esse comando é equivalente a criar uma transmissão usando o snap-in do MMC dos serviços de implantação do Windows (clique com o botão direito do mouse no nó **transmissões multicast** e clique em **criar transmissão multicast**). Você deve usar esse comando quando tiver o serviço de função servidor de implantação e o serviço de função de servidor de transporte instalados (que é a instalação padrão). Se você tiver apenas o serviço de função de servidor de transporte instalado, use o [comando wdsutilnew-namespace](wdsutil-new-namespace.md).
## <a name="syntax"></a>Syntax
para transmissões de imagens de instalação:
```
wdsutil [Options] /New-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
        /FriendlyName:<Friendly name>
        [/Description:<Description>]
        /Transmissiontype: {AutoCast | ScheduledCast}
            [/time:<YYYY/MM/DD:hh:mm>]
            [/Clients:<Num of Clients>]
      imagetype:Install
       ImageGroup:<Image Group>]
        [/Filename:<File name>]
```
para transmissões de imagem de inicialização (com suporte apenas para o Windows Server 2008 R2):
```
wdsutil [Options] /New-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
        /FriendlyName:<Friendly name>
        [/Description:<Description>]
        /Transmissiontype: {AutoCast | ScheduledCast}
            [/time:<YYYY/MM/DD:hh:mm>]
            [/Clients:<Num of Clients>]
      imagetype:Boot
        /Architecture:{x86 | ia64 | x64}
        [/Filename:<File name>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
| /Image<Image name>|Especifica o nome da imagem a ser transmitida usando multicast.|
|[/Server:<Server name>]|Especifica o nome do servidor. Esse pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|FriendlyName<Friendly name>|Especifica o nome amigável da transmissão.|
|[/Description:<Description>]|Especifica a descrição da transmissão.|
| /ImageType: {boot&#124;install}|Especifica o tipo de imagem a ser transmitida usando multicast. Observação só há suporte para a **inicialização** no Windows Server 2008 R2.|
| /ImageGroup: <Image group name> ]|Especifica o grupo de imagens que contém a imagem. Se nenhum nome de grupo de imagens for especificado e houver apenas um grupo de imagens no servidor, esse grupo de imagens será usado. Se houver mais de um grupo de imagens no servidor, você deverá usar essa opção para especificar o nome do grupo de imagens.|
|[/Filename:<File name>]|especifica o nome do arquivo. Se a imagem de origem não puder ser identificada exclusivamente pelo nome, você deverá usar essa opção para especificar o nome do arquivo.|
|/TransmissionType: {AutoCast &#124; ScheduledCast}|Especifica se a transmissão deve ser iniciada automaticamente (AutoCast) ou com base nos critérios de início especificados (ScheduledCast).<p><ul><li>**Conversão automática**. Esse tipo de transmissão indica que assim que um cliente aplicável solicitar uma imagem de instalação, uma transmissão multicast da imagem selecionada será iniciada. À medida que outros clientes solicitam a mesma imagem, eles são adicionados à transmissão que já foi iniciada.</li><li>**Com conversão agendada**. Esse tipo de transmissão define os critérios de início para a transmissão com base no número de clientes que estão solicitando uma imagem e/ou um dia e hora específicos. Você pode especificar as seguintes opções:<p><ul><li>[/time: <time> ] -Define a hora em que a transmissão deve começar usando o seguinte formato: AAAA/MM/DD: hh: mm.</li><li>[/Clients: <Number of clients> ] -Define o número mínimo de clientes a aguardar antes de iniciar a transmissão.</li></ul></li></ul>|
|/Architecture: {x86 &#124; IA64 &#124; x64}|Especifica a arquitetura da imagem de inicialização a ser transmitida usando multicast. Como é possível ter o mesmo nome para imagens de inicialização de diferentes arquiteturas, você deve especificar a arquitetura para garantir que a imagem correta seja usada.|
|[/Filename:<File name>]|especifica o nome do arquivo. Se a imagem de origem não puder ser identificada exclusivamente pelo nome, você deverá especificar o nome do arquivo.|
## <a name="examples"></a>Exemplos
Para criar uma transmissão de conversão automática de uma imagem de inicialização no Windows Server 2008 R2, digite:
```
wdsutil /New-MulticastTransmission /FriendlyName:WDS Boot Transmission
/Image:X64 Boot imagetype:Boot /Architecture:x64 /Transmissiontype:AutoCast
```
Para criar uma transmissão de conversão automática de uma imagem de instalação, digite:
```
wdsutil /New-MulticastTransmission /FriendlyName:WDS AutoCast Transmission
/Image:Vista with Officeimage imagetype:Install /Transmissiontype:AutoCast
```
Para criar uma transmissão de conversão agendada de uma imagem de instalação, digite:
```
wdsutil /New-MulticastTransmission /FriendlyName:WDS SchedCast Transmission /Server:MyWDSServer Image:Vista with Office imagetype:Install
/Transmissiontype:ScheduledCast /time:2006/11/20:17:00 /Clients:100
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [comando WDSUTIL Get-allmulticasttransmissions](wdsutil-get-allmulticasttransmissions.md)
- [comando WDSUTIL Get-MulticastTransmission](wdsutil-get-multicasttransmission.md)
- [comando WDSUTIL Remove-MulticastTransmission](wdsutil-remove-multicasttransmission.md)
- [comando WDSUTIL Start-MulticastTransmission](wdsutil-start-multicasttransmission.md)
