---
title: Subcomando start MulticastTransmission
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a1b2d459-1ece-49d4-997c-9d206c463b61
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d7e3e59a0907caf2769d5df00aeaf00589ab450d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842677"
---
# <a name="subcommand-start-multicasttransmission"></a>Subcommand: start-MulticastTransmission

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

inicia uma transmissão multicast programado de uma imagem.
## <a name="syntax"></a>Sintaxe
**Windows Server 2008**
```
wdsutil /start-MulticastTransmissiomedia:<Image name> [/Server:<Server namemediatype:InstallmediaGroup:<Image group name>] [/Filename:<File name>]
```
**Windows Server 2008 R2** para imagens de inicialização:
```
wdsutil [Options] /start-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
      mediatype:Boot
        /Architecture:{x86 | ia64 | x64}
        [/Filename:<File name>]
```
Para instalar imagens:
```
wdsutil [Options] /start-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
      mediatype:Install
       mediaGroup:<Image Group>]
        [/Filename:<File name>]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
mídia:<Image name>|Especifica o nome da imagem.|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
mediatype:{Install&#124;Boot}|Especifica o tipo de imagem. Observe que essa opção deve ser definida como **instalar** para Windows Server 2008.|
|/ Arquitetura: {x86 &#124; ia64 &#124; x64}|A arquitetura da imagem de inicialização que está associada com a transmissão para iniciar. Como é possível ter o mesmo nome de imagem para imagens de inicialização em arquiteturas diferentes, você deve especificar a arquitetura para garantir que a transmissão correta é usada.|
|\mediaGroup:<Image group name>]|Especifica o grupo de imagens da imagem. Se nenhum nome de grupo de imagem for especificado e o grupo de apenas uma imagem existe no servidor, esse grupo será usado. Se houver mais de um grupo de imagens no servidor, você deve usar essa opção para especificar o nome do grupo de imagem.|
|[/Filename:<File name>]|Especifica o nome do arquivo que contém a imagem. Se a imagem não pode ser identificada exclusivamente pelo nome, você deve usar essa opção para especificar o nome do arquivo.|
## <a name="BKMK_examples"></a>Exemplos
Para iniciar uma transmissão multicast, digite um dos seguintes:
```
wdsutil /start-MulticastTransmissiomedia:"Vista with Office"
/Imagetype:Install
wdsutil /start-MulticastTransmission /Server:MyWDSServemedia:"Vista with Officemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim
```
Para iniciar uma inicialização de transmissão multicast para Windows Server 2008 R2, o tipo de imagem:
```
wdsutil /start-MulticastTransmission /Server:MyWDSServemedia:"X64 Boot Imagemediatype:Boot /Architecture:x64
/Filename:boot.wim\n\
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando get-AllMulticastTransmissions](using-the-get-allmulticasttransmissions-command.md)
[usando o comando get-MulticastTransmission](using-the-get-multicasttransmission-command.md) 
 [Usando o comando /New-MulticastTransmission](using-the-new-multicasttransmission-command.md)
[usando o comando remove-MulticastTransmission](using-the-remove-multicasttransmission-command.md)
