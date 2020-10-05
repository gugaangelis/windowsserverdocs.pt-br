---
title: WDSUTIL Remove-namespace
description: Artigo de referência para WDSUTIL Remove-namespace, que remove um namespace personalizado.
ms.topic: reference
ms.assetid: 4eb758b6-8519-4e26-9fe0-2e19bb0e8702
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: bc5d44b727bf93bf6630f7289833640397e69dae
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729822"
---
# <a name="wdsutil-remove-namespace"></a>WDSUTIL Remove-namespace

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Remove um namespace personalizado.

## <a name="syntax"></a>Sintaxe
```
wdsutil /remove-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/force]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|Namespace<Namespace name>|Especifica o nome do namespace. Esse não é o nome amigável e deve ser exclusivo.<p>-   **Serviço de função do servidor de implantação**: a sintaxe para o nome do namespace é/namespace: WDS: <ImageGroup> / <ImageName> / <Index> . Por exemplo: **WDS: ImageGroup1/install. wim/1**<br />-   **Serviço de função do servidor de transporte**: esse valor deve corresponder ao nome fornecido para o namespace quando ele foi criado no servidor.|
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
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [comando WDSUTIL Get-MyNamespaces](wdsutil-get-allnamespaces.md)
- [comando WDSUTIL New-namespace](wdsutil-new-namespace.md)
- [comando Start-namespace WDSUTIL](wdsutil-start-namespace.md)
