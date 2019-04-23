---
title: Usando o comando Novo Namespace
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6df60703-30bd-4d59-a8d9-9fe3efe96add
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 50d51101afe95c99b7034fc50b3d30b799ee02ce
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871147"
---
# <a name="using-the-new-namespace-command"></a>Usando o comando Novo Namespace

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

cria e configura um novo namespace. Você deve usar essa opção quando você tem apenas o serviço de função de servidor de transporte instalado. Se você tiver instalado o serviço de função de servidor de implantação e o serviço de função de servidor de transporte que (que é o padrão), use [usando o comando /New-MulticastTransmission](using-the-new-multicasttransmission-command.md). Observe que você deve registrar o provedor de conteúdo antes de usar essa opção.
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
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Isso pode ser o nome NetBIOS ou o nome de domínio totalmente qualificado (FQDN). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|/FriendlyName:<Friendly name>|Especifica o nome amigável do namespace.|
|[/Description:<Description>]|Define a descrição do namespace.|
|/Namespace:<Namespace name>|Especifica o nome do namespace. Observe que isso não é o nome amigável, e ele deve ser exclusivo.<br /><br />-   **Serviço de função de servidor de implantação**: A sintaxe para essa opção é /Namespace:WDS:<Image group>/<Image name>/<Index>. Por exemplo:  **WDS:ImageGroup1/install.wim/1**<br />-   **Serviço de função de servidor de transporte**: Esse valor deve corresponder ao nome fornecido quando o namespace foi criado no servidor.|
|/ContentProvider:<Name>]|Especifica o nome do provedor de conteúdo que fornecerá o conteúdo para o namespace.|
|[/ConfigString:<Configuration string>]|Especifica a cadeia de caracteres de configuração para o provedor de conteúdo.|
|/Namespacetype: {AutoCast &#124; ScheduledCast}|Especifica as configurações para a transmissão. Você especifica as configurações usando as seguintes opções:<br /><br />-[/ hora: <time>]-define a hora em que a transmissão deve começar usando o seguinte formato: AAAA/MM/DD:hh:mm. Essa opção só se aplica a transmissões multicast agendadas.<br />-[/ Clientes: <Number of clients>]-define o número mínimo de clientes para aguardar antes de inicia a transmissão. Essa opção só se aplica a transmissões multicast agendadas.|
## <a name="BKMK_examples"></a>Exemplos
Para criar um namespace de multicast automático, digite:
```
wdsutil /New-Namespace /FriendlyName:"Custom AutoCast Namespace" /Namespace:"Custom Auto 1" /ContentProvider:MyContentProvider /Namespacetype:AutoCast
```
Para criar um namespace de multicast programado, digite:
```
wdsutil /New-Namespace /Server:MyWDSServer /FriendlyName:"Custom Scheduled Namespace" /Namespace:"Custom Auto 1" /ContentProvider:MyContentProvider 
/Namespacetype:ScheduledCast /time:"2006/11/20:17:00" /Clients:20
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando get-AllNamespaces](using-the-get-allnamespaces-command.md)
[usando o comando remove-Namespace](using-the-remove-namespace-command.md) 
 [ Subcomando: start-Namespace](subcommand-start-namespace.md)
