---
title: remover namespace
description: Tópico de referência para Remove-namespace, que remove um namespace personalizado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4eb758b6-8519-4e26-9fe0-2e19bb0e8702
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ef329a53a7f212c1810af2e4ced11a2abf76840
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720331"
---
# <a name="using-the-remove-namespace-command"></a>Usando o comando Remove-namespace

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Remove um namespace personalizado.

## <a name="syntax"></a>Sintaxe
```
wdsutil /remove-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/force]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|Namespace<Namespace name>|Especifica o nome do namespace. Esse não é o nome amigável e deve ser exclusivo.<p>-   **Serviço de função do servidor de implantação**: a sintaxe para o nome do namespace<ImageGroup>/<ImageName>/<Index>é/namespace: WDS:. Por exemplo: **WDS: ImageGroup1/install. wim/1**<br />-   **Serviço de função do servidor de transporte**: esse valor deve corresponder ao nome fornecido para o namespace quando ele foi criado no servidor.|
|[/Server:<Server name>]|Especifica o nome do servidor. Esse pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|/Force|Remove o namespace imediatamente e encerra todos os clientes. Observe que, a menos que você especifique **/Force**, os clientes existentes poderão concluir a transferência, mas novos clientes não poderão ingressar.|
## <a name="examples"></a>Exemplos
Para interromper um namespace (os clientes atuais podem concluir a transferência, mas novos clientes não podem ingressar), digite:
```
wdsutil /remove-Namespace /Namespace:Custom Auto 1
```
Para forçar o encerramento de todos os clientes, digite:
```
wdsutil /remove-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1 /force
```
## <a name="additional-references"></a>Referências adicionais
- [Chave](command-line-syntax-key.md)
de sintaxe de linha de comando
[usando o comando Get-MyNamespaces](using-the-get-allnamespaces-command.md)usando o subcomando de
[comando New-namespace](using-the-new-namespace-command.md)[: Start-namespace](subcommand-start-namespace.md)
