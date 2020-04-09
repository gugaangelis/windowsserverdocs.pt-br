---
title: novo namespace
description: Tópico de comandos do Windows para New-namespace, que cria e configura um novo namespace.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6df60703-30bd-4d59-a8d9-9fe3efe96add
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6f616c33525d033827d52925e07764761e7c7b44
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830629"
---
# <a name="new-namespace"></a>novo namespace

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria e configura um novo namespace. Você deve usar essa opção quando tiver apenas o serviço de função de servidor de transporte instalado. Se você tiver o serviço de função servidor de implantação e o serviço de função de servidor de transporte instalados (que é o padrão), use [o comando New-MulticastTransmission](using-the-new-multicasttransmission-command.md). Observe que você deve registrar o provedor de conteúdo antes de usar essa opção.
## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /New-Namespace [/Server:<Server name>]
     /FriendlyName:<Friendly name>
     [/Description:<Description>]
     /Namespace:<Namespace name>
     /ContentProvider:<Name>
     [/ConfigString:<Configuration string>]
     /Namespacetype: {AutoCast | ScheduledCast}
         [/time:<YYYY/MM/DD:hh:mm>]
         [/Clients:<Number of clients>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Esse pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|/FriendlyName:<Friendly name>|Especifica o nome amigável do namespace.|
|[/Description:<Description>[]|Define a descrição do namespace.|
|/Namespace:<Namespace name>|Especifica o nome do namespace. Observe que esse não é o nome amigável e deve ser exclusivo.<p>-   **serviço de função do servidor de implantação**: a sintaxe dessa opção é/namespace: WDS:<Image group>/<Image name>/<Index>. Por exemplo: **WDS: ImageGroup1/install. wim/1**<br />**serviço de função de servidor de transporte**-   : esse valor deve corresponder ao nome fornecido quando o namespace foi criado no servidor.|
|/ContentProvider:<Name>]|Especifica o nome do provedor de conteúdo que fornecerá conteúdo para o namespace.|
|[/ConfigString:<Configuration string>]|Especifica a cadeia de caracteres de configuração para o provedor de conteúdo.|
|/NamespaceType: {AutoCast &#124; ScheduledCast}|Especifica as configurações para a transmissão. Você especifica as configurações usando as seguintes opções:<p>-[/time: <time>] – define a hora em que a transmissão deve começar usando o seguinte formato: AAAA/MM/DD: hh: mm. Esta opção se aplica somente a transmissões de conversão agendada.<br />-[/Clients: <Number of clients>] – define o número mínimo de clientes a aguardar antes de iniciar a transmissão. Esta opção se aplica somente a transmissões de conversão agendada.|
## <a name="examples"></a><a name=BKMK_examples></a>Disso
Para criar um namespace de conversão automática, digite:
```
wdsutil /New-Namespace /FriendlyName:Custom AutoCast Namespace /Namespace:Custom Auto 1 /ContentProvider:MyContentProvider /Namespacetype:AutoCast
```
Para criar um namespace de elenco agendado, digite:
```
wdsutil /New-Namespace /Server:MyWDSServer /FriendlyName:Custom Scheduled Namespace /Namespace:Custom Auto 1 /ContentProvider:MyContentProvider 
/Namespacetype:ScheduledCast /time:2006/11/20:17:00 /Clients:20
```
## <a name="additional-references"></a>Referências adicionais
- A [chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando Get-mynamespaces](using-the-get-allnamespaces-command.md)
[usando o comando Remove-namespace](using-the-remove-namespace-command.md)
[subcomando: Start-namespace](subcommand-start-namespace.md)
