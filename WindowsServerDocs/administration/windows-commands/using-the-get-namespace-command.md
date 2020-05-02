---
title: Get-namespace
description: Tópico de referência para Get-namespace, que exibe informações sobre um namespace personalizado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ea641bab-e97b-4909-918e-447730027dc1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 76980d2add9ee9b7584812c9d366408f8770b681
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719750"
---
# <a name="get-namespace"></a>Get-namespace

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe informações sobre um namespace personalizado.

## <a name="syntax"></a>Sintaxe
Windows Server 2008 R2
```
wdsutil /Get-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/Show:Clients]
```
Windows Server 2008 R2
```
wdsutil /Get-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/details:Clients]
```
### <a name="parameters"></a>Parâmetros

|               Parâmetro               |                                                                                                                                                                                         Descrição                                                                                                                                                                                          |
|---------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      Namespace<Namespace name>      | Especifica o nome do namespace. Observe que esse não é o nome amigável e deve ser exclusivo.<p>-Servidor de implantação: a sintaxe para o nome do namespace é/namspace<ImageGroup>/<ImageName>/<Index>: WDS:. Por exemplo: **WDS: ImageGroup1/install. wim/1**<br />-Servidor de transporte: esse valor deve corresponder ao nome fornecido para o namespace quando ele foi criado no servidor. |
|        [/Server:<Server name>]        |                                                                                                             Especifica o nome do servidor. Esse pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.                                                                                                              |
| [/Show: clients] ou [/details: clients] |                                                                                                                                                  Exibe informações sobre os computadores cliente que estão conectados ao namespace especificado.                                                                                                                                                  |

## <a name="examples"></a>Exemplos
Para exibir informações sobre um namespace, digite:
```
wdsutil /Get-Namespace /Namespace:Custom Auto 1
```
Para exibir informações sobre um namespace e os clientes que estão conectados, digite um dos seguintes:
- Windows Server 2008:`wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1 /Show:Clients`
- Windows Server 2008 R2:`wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1 /details:Clients`
  ## <a name="additional-references"></a>Referências adicionais
  - [Chave](command-line-syntax-key.md)
  de sintaxe de linha de comando[usando o comando](using-the-get-allnamespaces-command.md)
  get-
  MyNamespaces[usando o comando New-namespace](using-the-new-namespace-command.md)[usando o subcomando remove-namespace Command](using-the-remove-namespace-command.md)
  [: Start-namespace](subcommand-start-namespace.md)
