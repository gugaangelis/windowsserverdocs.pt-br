---
title: remover namespace
description: Tópico de comandos do Windows para Remove-namespace, que remove um namespace personalizado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4eb758b6-8519-4e26-9fe0-2e19bb0e8702
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1f8b830a5d99d13ed00a3a19f2cf246ad71d1c5f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830229"
---
# <a name="using-the-remove-namespace-command"></a>Usando o comando Remove-namespace

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Remove um namespace personalizado.

## <a name="syntax"></a>Sintaxe
```
wdsutil /remove-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/force]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/Namespace:<Namespace name>|Especifica o nome do namespace. Esse não é o nome amigável e deve ser exclusivo.<p>-   **serviço de função do servidor de implantação**: a sintaxe para o nome do namespace é/namespace: WDS:<ImageGroup>/<ImageName>/<Index>. Por exemplo: **WDS: ImageGroup1/install. wim/1**<br />**serviço de função de servidor de transporte**-   : esse valor deve corresponder ao nome fornecido para o namespace quando ele foi criado no servidor.|
|[/Server:<Server name>]|Especifica o nome do servidor. Esse pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|/Force|Remove o namespace imediatamente e encerra todos os clientes. Observe que, a menos que você especifique **/Force**, os clientes existentes poderão concluir a transferência, mas novos clientes não poderão ingressar.|
## <a name="examples"></a><a name=BKMK_examples></a>Disso
Para interromper um namespace (os clientes atuais podem concluir a transferência, mas novos clientes não podem ingressar), digite:
```
wdsutil /remove-Namespace /Namespace:Custom Auto 1
```
Para forçar o encerramento de todos os clientes, digite:
```
wdsutil /remove-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1 /force
```
## <a name="additional-references"></a>Referências adicionais
- A [chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando Get-mynamespaces](using-the-get-allnamespaces-command.md)
[usando o comando New-namespace](using-the-new-namespace-command.md)
[subcomando: Start-namespace](subcommand-start-namespace.md)
