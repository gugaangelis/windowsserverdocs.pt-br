---
title: Usando o comando remove-MulticastTransmission
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9a7f5c31-bfbf-425d-9129-a6f9173fe83d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dc3ba385644ef9da9b5d592142091ff087cd7545
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839677"
---
# <a name="using-the-remove-multicasttransmission-command"></a>Usando o comando remove-MulticastTransmission

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Desabilita a transmissão multicast para uma imagem. A menos que você especifique **/Force**, os clientes existentes serão concluir a transferência de imagem, mas novos clientes serão permitidos para ingressar.
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
Para instalar imagens:
```
wdsutil [Options] /remove-MulticastTransmissiomedia:<Image name>
        [/Server:<Server name>]
      mediatype:Install
       mediaGroup:<Image Group
        [/Filename:<File name>]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
mídia:<Image name>|Especifica o nome da imagem.|
|[/Server:<Server name>]|Especifica o nome do servidor. Isso pode ser o nome NetBIOS ou o nome de domínio totalmente qualificado (FQDN). Se nenhum nome de servidor for especificado, o servidor local será usado.|
mediatype:{Install&#124;Boot}|Especifica o tipo de imagem. Observe que essa opção deve ser definida como **instalar** para Windows Server 2008.|
|/ Arquitetura: {x86 &#124; ia64 &#124; x64}|Especifica a arquitetura da imagem de inicialização que está associada com a transmissão para iniciar. Como é possível ter o mesmo nome de imagem para imagens de inicialização em arquiteturas diferentes, você deve especificar a arquitetura para garantir que a transmissão correta é usada.|
|\mediaGroup:<Image group name>]|Especifica o grupo de imagens que contém a imagem. Se nenhum nome de grupo de imagem for especificado e o grupo de apenas uma imagem existe no servidor, esse grupo é usado. Se houver mais de um grupo de imagens no servidor, você deve usar essa opção para especificar o nome do grupo de imagem.|
|[/Filename:<File name>]|Especifica o nome do arquivo. Se a imagem de origem não pode ser identificada exclusivamente pelo nome, você deve usar essa opção para especificar o nome do arquivo.|
|[/force]|Remove a transmissão e encerra todos os clientes. A menos que você especifique um valor para o **/Force** opção existente os clientes pode concluir a transferência de imagem, mas novos clientes não são capazes de ingressar.|
## <a name="BKMK_examples"></a>Exemplos
Para interromper um namespace (clientes atuais serão concluída a transmissão, mas novos clientes não poderão ingressar), digite:
```
wdsutil /remove-MulticastTransmissiomedia:"Vista with Office"
/Imagetype:Install
```
```
wdsutil /remove-MulticastTransmissiomedia:"x64 Boot Image"
/Imagetype:Boot /Architecture:x64
```
Para forçar o encerramento de todos os clientes, digite:
```
wdsutil /remove-MulticastTransmission /Server:MyWDSServer
/Image:"Vista with Officemediatype:InstalmediaGroup:ImageGroup1
/Filename:install.wim /force
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando get-AllMulticastTransmissions](using-the-get-allmulticasttransmissions-command.md)
[usando o comando get-MulticastTransmission](using-the-get-multicasttransmission-command.md) 
 [Usando o comando /New-MulticastTransmission](using-the-new-multicasttransmission-command.md)
[subcomando: início MulticastTransmission](subcommand-start-multicasttransmission.md)
