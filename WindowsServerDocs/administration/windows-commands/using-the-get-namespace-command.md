---
title: Get-namespace
description: Artigo de referência para Get-namespace, que exibe informações sobre um namespace personalizado.
ms.topic: article
ms.assetid: ea641bab-e97b-4909-918e-447730027dc1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e998334b8297b06bf5eb23b9106acd3770504ffb
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896931"
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
|      Namespace<Namespace name>      | Especifica o nome do namespace. Observe que esse não é o nome amigável e deve ser exclusivo.<p>-Servidor de implantação: a sintaxe para o nome do namespace é/namspace: WDS: <ImageGroup> / <ImageName> / <Index> . Por exemplo: **WDS: ImageGroup1/install. wim/1**<br />-Servidor de transporte: esse valor deve corresponder ao nome fornecido para o namespace quando ele foi criado no servidor. |
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
  - Chave de sintaxe [de linha de comando](command-line-syntax-key.md) 
   [Usando o comando](using-the-get-allnamespaces-command.md) 
   Get-MyNamespaces [Usando o comando](using-the-new-namespace-command.md) 
   New-namespace [Usando o comando](using-the-remove-namespace-command.md) 
   Remove-namespace [Subcomando: Start-namespace](subcommand-start-namespace.md)
