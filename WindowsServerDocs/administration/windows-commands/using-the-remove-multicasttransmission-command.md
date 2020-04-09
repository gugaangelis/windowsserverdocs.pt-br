---
title: Remove-MulticastTransmission
description: O tópico de comandos do Windows para Remove-MulticastTransmission, que desabilita a transmissão multicast para uma imagem.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9a7f5c31-bfbf-425d-9129-a6f9173fe83d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 36bb666b841c4c0c33f12c5ca766e619afc176ef
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830329"
---
# <a name="using-the-remove-multicasttransmission-command"></a>Usando o comando Remove-MulticastTransmission

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Desabilita a transmissão multicast para uma imagem. A menos que você especifique **/Force**, os clientes existentes concluirão a transferência de imagem, mas novos clientes não terão permissão para ingressar.

## <a name="syntax"></a>Sintaxe
**Windows Server 2008**
```
wdsutil /remove-MulticastTransmissiomedia:<Image name> [/Server:<Server name>mediatype:InstallmediaGroup:<Image Group>] [/Filename:<File name>] [/force]
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
mídia:<Image name>|Especifica o nome da imagem.|
|[/Server:<Server name>]|Especifica o nome do servidor. Esse pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
MediaType: {instalar&#124;inicialização}|Especifica o tipo de imagem. Observe que essa opção deve ser definida como **instalar** para o Windows Server 2008.|
|/Architecture: {x86 &#124; IA64 &#124; x64}|Especifica a arquitetura da imagem de inicialização associada à transmissão a ser iniciada. Como é possível ter o mesmo nome de imagem para imagens de inicialização em diferentes arquiteturas, você deve especificar a arquitetura para garantir que a transmissão correta seja usada.|
|\mediaGroup:<Image group name>]|Especifica o grupo de imagens que contém a imagem. Se nenhum nome de grupo de imagens for especificado e houver apenas um grupo de imagens no servidor, esse grupo de imagens será usado. Se houver mais de um grupo de imagens no servidor, você deverá usar essa opção para especificar o nome do grupo de imagens.|
|[/Filename:<File name>]|Especifica o nome do arquivo. Se a imagem de origem não puder ser identificada exclusivamente pelo nome, você deverá usar essa opção para especificar o nome do arquivo.|
|/Force|Remove a transmissão e encerra todos os clientes. A menos que você especifique um valor para a opção **/Force** , os clientes existentes podem concluir a transferência de imagem, mas novos clientes não podem ingressar.|
## <a name="examples"></a><a name=BKMK_examples></a>Disso
Para interromper um namespace (os clientes atuais concluirão a transmissão, mas novos clientes não poderão ingressar), digite:
```
wdsutil /remove-MulticastTransmissiomedia:Vista with Office
/Imagetype:Install
```
```
wdsutil /remove-MulticastTransmissiomedia:x64 Boot Image
/Imagetype:Boot /Architecture:x64
```
Para forçar o encerramento de todos os clientes, digite:
```
wdsutil /remove-MulticastTransmission /Server:MyWDSServer
/Image:Vista with Officemediatype:InstalmediaGroup:ImageGroup1
/Filename:install.wim /force
```
## <a name="additional-references"></a>Referências adicionais
- A [chave de sintaxe de linha de comando](command-line-syntax-key.md)
usando o [comando get-AllMulticastTransmissions](using-the-get-allmulticasttransmissions-command.md)
[usando o comando Get-MulticastTransmission](using-the-get-multicasttransmission-command.md)
[usando o comando New-MulticastTransmission
o](using-the-new-multicasttransmission-command.md) [subcomando: Start-MulticastTransmission](subcommand-start-multicasttransmission.md)
