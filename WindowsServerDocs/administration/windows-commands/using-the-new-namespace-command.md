---
title: novo namespace
description: Artigo de referência para New-namespace, que cria e configura um novo namespace.
ms.topic: reference
ms.assetid: 6df60703-30bd-4d59-a8d9-9fe3efe96add
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8e9bcece219117c559c298bb97726d60c11faa44
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038134"
---
# <a name="new-namespace"></a>novo namespace

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
|FriendlyName<Friendly name>|Especifica o nome amigável do namespace.|
|[/Description:<Description>]|Define a descrição do namespace.|
|Namespace<Namespace name>|Especifica o nome do namespace. Observe que esse não é o nome amigável e deve ser exclusivo.<p>-   **Serviço de função do servidor de implantação**: a sintaxe dessa opção é/namespace: WDS: <Image group> / <Image name> / <Index> . Por exemplo: **WDS: ImageGroup1/install. wim/1**<br />-   **Serviço de função do servidor de transporte**: esse valor deve corresponder ao nome fornecido quando o namespace foi criado no servidor.|
|/ContentProvider: <Name> ]|Especifica o nome do provedor de conteúdo que fornecerá conteúdo para o namespace.|
|[/ConfigString: <Configuration string> ]|Especifica a cadeia de caracteres de configuração para o provedor de conteúdo.|
|/NamespaceType: {AutoCast &#124; ScheduledCast}|Especifica as configurações para a transmissão. Você especifica as configurações usando as seguintes opções:<p>-[/time: <time> ] – define a hora em que a transmissão deve começar usando o seguinte formato: aaaa/mm/dd: hh: mm. Esta opção se aplica somente a transmissões de conversão agendada.<br />-[/Clients: <Number of clients> ] – define o número mínimo de clientes a aguardar antes de iniciar a transmissão. Esta opção se aplica somente a transmissões de conversão agendada.|
## <a name="examples"></a>Exemplos
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
- Chave de sintaxe [de linha de comando](command-line-syntax-key.md) 
 [Usando o comando](using-the-get-allnamespaces-command.md) 
 Get-MyNamespaces [Usando o comando](using-the-remove-namespace-command.md) 
 Remove-namespace [Subcomando: Start-namespace](subcommand-start-namespace.md)
