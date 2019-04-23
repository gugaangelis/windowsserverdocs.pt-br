---
title: Usando o comando /New-MulticastTransmission
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c1f1dc46-dd50-4eb9-9f72-cf0e5d71bd3d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 84f5aa69f2d4a875995ac6c18fa43bd68518fba3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875137"
---
# <a name="using-the-new-multicasttransmission-command"></a>Usando o comando /New-MulticastTransmission

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

cria uma nova transmissão multicast para uma imagem. Esse comando é equivalente a criar uma transmissão usando o snap-in do mmc dos serviços de implantação do Windows (com o botão direito do **transmissões Multicast** nó e, em seguida, clique **criar transmissão Multicast**). Você deve usar esse comando quando você tem o serviço de função de servidor de implantação e o servidor de transporte serviço de função instalado (que é a instalação padrão). Se você tiver somente o serviço de função de servidor de transporte instalado, use [usando o comando Novo Namespace](using-the-new-namespace-command.md).
## <a name="syntax"></a>Sintaxe
para as transmissões de imagens de instalação:
```
wdsutil [Options] /New-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
        /FriendlyName:<Friendly name>
        [/Description:<Description>]
        /Transmissiontype: {AutoCast | ScheduledCast}
            [/time:<YYYY/MM/DD:hh:mm>]
            [/Clients:<Num of Clients>]
      mediatype:Install
       mediaGroup:<Image Group>]
        [/Filename:<File name>]
```
para transmissões de imagem de inicialização (somente suportadas para Windows Server 2008 R2):
```
wdsutil [Options] /New-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
        /FriendlyName:<Friendly name>
        [/Description:<Description>]
        /Transmissiontype: {AutoCast | ScheduledCast}
            [/time:<YYYY/MM/DD:hh:mm>]
            [/Clients:<Num of Clients>]
      mediatype:Boot
        /Architecture:{x86 | ia64 | x64}
        [/Filename:<File name>]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
mídia:<Image name>|Especifica o nome da imagem a ser transmitido usando multicast.|
|[/Server:<Server name>]|Especifica o nome do servidor. Isso pode ser o nome NetBIOS ou o nome de domínio totalmente qualificado (FQDN). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|/FriendlyName:<Friendly name>|Especifica o nome amigável da transmissão.|
|[/Description:<Description>]|Especifica a descrição da transmissão.|
mediatype:{Boot&#124;Install}|Especifica o tipo de imagem a ser transmitido usando multicast. Observação **inicialização** só tem suporte para o Windows Server 2008 R2.|
|\mediaGroup:<Image group name>]|Especifica o grupo de imagens que contém a imagem. Se nenhum nome de grupo de imagem for especificado e o grupo de apenas uma imagem existe no servidor, esse grupo é usado. Se houver mais de um grupo de imagens no servidor, você deve usar essa opção para especificar o nome do grupo de imagem.|
|[/Filename:<File name>]|Especifica o nome do arquivo. Se a imagem de origem não pode ser identificada exclusivamente pelo nome, você deve usar essa opção para especificar o nome do arquivo.|
|/Transmissiontype:{AutoCast &#124; ScheduledCast}|Especifica se deve iniciar a transmissão automaticamente (AutoCast) ou com base em critérios de início especificada (multicast).<br /><br /><ul><li>**Multicast automático**. Esse tipo de transmissão indica que um cliente aplicável solicitar uma imagem de instalação, assim que uma transmissão multicast da imagem selecionada começa. Como a mesma imagem de solicitação de outros clientes, elas são unidas a transmissão que já foi iniciado.</li><li>**O multicast programado**. Esse tipo de transmissão define os critérios de início para a transmissão com base no número de clientes que estão solicitando uma imagem e/ou um dia e hora específicos. Você pode especificar as seguintes opções:<br /><br /><ul><li>[/ hora: <time>]-define a hora em que a transmissão deve começar usando o seguinte formato: AAAA/MM/DD:hh:mm.</li><li>[/ Clientes: <Number of clients>]-define o número mínimo de clientes para aguardar antes de inicia a transmissão.</li></ul></li></ul>|
|/ Arquitetura: {x86 &#124; ia64 &#124; x64}|Especifica a arquitetura da imagem de inicialização para transmitir usando multicasting. Como é possível ter o mesmo nome para as imagens de inicialização de arquiteturas diferentes, você deve especificar a arquitetura para garantir que a imagem correta é usada.|
|[/Filename:<File name>]|Especifica o nome do arquivo. Se a imagem de origem não pode ser identificada exclusivamente pelo nome, você deve especificar o nome do arquivo.|
## <a name="BKMK_examples"></a>Exemplos
Para criar uma transmissão multicast automático de uma imagem de inicialização no Windows Server 2008 R2, digite:
```
wdsutil /New-MulticastTransmission /FriendlyName:"WDS Boot Transmission"
/Image:"X64 Boot Imagemediatype:Boot /Architecture:x64 /Transmissiontype:AutoCast
```
Para criar uma transmissão multicast automático de uma imagem de instalação, digite:
```
wdsutil /New-MulticastTransmission /FriendlyName:"WDS AutoCast Transmission"
/Image:"Vista with Officemediatype:Install /Transmissiontype:AutoCast
```
Para criar uma transmissão de multicast programado de uma imagem de instalação, digite:
```
wdsutil /New-MulticastTransmission /FriendlyName:"WDS SchedCast Transmission" /Server:MyWDSServemedia:"Vista with Officemediatype:Install 
/Transmissiontype:ScheduledCast /time:"2006/11/20:17:00" /Clients:100
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando get-AllMulticastTransmissions](using-the-get-allmulticasttransmissions-command.md)
[usando o comando get-MulticastTransmission](using-the-get-multicasttransmission-command.md) 
 [Usando o comando remove-MulticastTransmission](using-the-remove-multicasttransmission-command.md)
[subcomando: início MulticastTransmission](subcommand-start-multicasttransmission.md)
