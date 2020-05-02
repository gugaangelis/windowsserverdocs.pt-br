---
title: Início do subcomando-MulticastTransmission
description: Tópico de referência para o subcomando Start-MulticastTransmission, que inicia uma transmissão de conversão agendada de uma imagem.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a1b2d459-1ece-49d4-997c-9d206c463b61
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5f3ea615da5aa48e805b3b5e3d0df0a02198a304
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721674"
---
# <a name="subcommand-start-multicasttransmission"></a>Subcomando: Start-MulticastTransmission

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Inicia uma transmissão de conversão agendada de uma imagem.

## <a name="syntax"></a>Sintaxe
**Windows Server 2008**
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
para imagens de instalação:
```
wdsutil [Options] /start-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
      mediatype:Install
       mediaGroup:<Image Group>]
        [/Filename:<File name>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
meio<Image name>|Especifica o nome da imagem.|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
MediaType: {instalar&#124;inicialização}|Especifica o tipo de imagem. Observe que essa opção deve ser definida como **instalar** para o Windows Server 2008.|
|/Architecture: {x86 &#124; IA64 &#124; x64}|A arquitetura da imagem de inicialização associada à transmissão para iniciar. Como é possível ter o mesmo nome de imagem para imagens de inicialização em diferentes arquiteturas, você deve especificar a arquitetura para garantir que a transmissão correta seja usada.|
|\mediaGroup:<Image group name>]|Especifica o grupo de imagens da imagem. Se nenhum nome de grupo de imagens for especificado e houver apenas um grupo de imagens no servidor, esse grupo de imagens será usado. Se houver mais de um grupo de imagens no servidor, você deverá usar essa opção para especificar o nome do grupo de imagens.|
|[/Filename:<File name>]|Especifica o nome do arquivo que contém a imagem. Se a imagem não puder ser identificada exclusivamente pelo nome, você deverá usar essa opção para especificar o nome do arquivo.|
## <a name="examples"></a>Exemplos
Para iniciar uma transmissão multicast, digite uma das seguintes opções:
```
wdsutil /start-MulticastTransmissiomedia:Vista with Office
/Imagetype:Install
wdsutil /start-MulticastTransmission /Server:MyWDSServemedia:Vista with Officemediatype:InstalmediaGroup:ImageGroup1 /Filename:install.wim
```
Para iniciar uma transmissão de multicast de imagem de inicialização para o Windows Server 2008 R2, digite:
```
wdsutil /start-MulticastTransmission /Server:MyWDSServemedia:X64 Boot Imagemediatype:Boot /Architecture:x64
/Filename:boot.wim\n\
```
## <a name="additional-references"></a>Referências adicionais
- [Chave](command-line-syntax-key.md)
de sintaxe de linha de comando usando o comando[Get-AllMulticastTransmissions](using-the-get-allmulticasttransmissions-command.md)
usando o

[comando Get-MulticastTransmission](using-the-get-multicasttransmission-command.md)[usando o comando New-MulticastTransmission](using-the-new-multicasttransmission-command.md)[usando o comando Remove-MulticastTransmission](using-the-remove-multicasttransmission-command.md)
