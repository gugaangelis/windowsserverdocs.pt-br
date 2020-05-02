---
title: Início do subcomando-namespace
description: Tópico de referência para o subcomando Start-namespace, que inicia um namespace de conversão agendada.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2dd1c11e-6ab7-4129-9e3a-3f80e0ba59c0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1562fcb6c61533fcc9994e9011bf7d61154c06f7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721663"
---
# <a name="subcommand-start-namespace"></a>Subcomando: Start-namespace

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Inicia um namespace de conversão agendada.

## <a name="syntax"></a>Sintaxe
```
wdsutil /start-Namespace /Namespace:<Namespace name[/Server:<Server name>]
```
### <a name="parameters"></a>Parâmetros

|          Parâmetro          |                                                                                                                                                                                             Descrição                                                                                                                                                                                             |
|-----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /Namespace: nome do namespace de <| Especifica o nome do namespace. Observe que esse não é o nome amigável e deve ser exclusivo.<p>-   **Servidor de implantação**: a sintaxe para o nome do namespace é/NAMSPACE<Image group>/<Image name>/<Index>: WDS:. Por exemplo: **WDS: ImageGroup1/install. wim/1**<br />-   **Servidor de transporte**: esse nome deve corresponder ao nome fornecido para o namespace quando ele foi criado no servidor. |
|   [/Server:<Server name>]   |                                                                                                           Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.                                                                                                           |

## <a name="examples"></a>Exemplos
Para iniciar um namespace, digite um dos seguintes:
```
wdsutil /start-Namespace /Namespace:Custom Auto 1
wdsutil /start-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1
```
## <a name="additional-references"></a>Referências adicionais
- [Chave](command-line-syntax-key.md)
de sintaxe de linha de comando usando o
[comando Get-MyNamespaces](using-the-get-allnamespaces-command.md)
[usando o comando New-namespace](using-the-new-namespace-command.md)[usando o comando Remove-namespace](using-the-remove-namespace-command.md)
