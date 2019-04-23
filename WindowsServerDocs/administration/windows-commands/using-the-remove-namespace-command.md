---
title: Usando o comando remove-Namespace
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4eb758b6-8519-4e26-9fe0-2e19bb0e8702
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 115c0a90a60e18ee4b89758200773d1dfec2163f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842037"
---
# <a name="using-the-remove-namespace-command"></a>Usando o comando remove-Namespace

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Remove um espaço para nome personalizado.
## <a name="syntax"></a>Sintaxe
```
wdsutil /remove-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/force]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/Namespace:<Namespace name>|Especifica o nome do namespace. Isso não é o nome amigável, e ele deve ser exclusivo.<br /><br />-   **Serviço de função de servidor de implantação**: A sintaxe de nome de namespace é /Namespace:WDS:<ImageGroup>/<ImageName>/<Index>. Por exemplo: **WDS:ImageGroup1/install.wim/1**<br />-   **Serviço de função de servidor de transporte**: Esse valor deve corresponder ao nome fornecido para o namespace quando ele foi criado no servidor.|
|[/Server:<Server name>]|Especifica o nome do servidor. Isso pode ser o nome NetBIOS ou o nome de domínio totalmente qualificado (FQDN). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|[/force]|Remove o namespace imediatamente e encerra todos os clientes. Observe que, a menos que você especifique **/Force**, os clientes existentes podem concluir a transferência, mas novos clientes não são capazes de ingressar.|
## <a name="BKMK_examples"></a>Exemplos
Para interromper um namespace (clientes atuais podem concluir a transferência, mas novos clientes não são capazes de ingressar), digite:
```
wdsutil /remove-Namespace /Namespace:"Custom Auto 1"
```
Para forçar o encerramento de todos os clientes, digite:
```
wdsutil /remove-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1" /force
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando get-AllNamespaces](using-the-get-allnamespaces-command.md)
[usando o comando Novo Namespace](using-the-new-namespace-command.md) 
 [ Subcomando: start-Namespace](subcommand-start-namespace.md)
