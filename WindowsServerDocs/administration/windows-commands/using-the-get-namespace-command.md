---
title: Get-namespace
description: O tópico de comandos do Windows para Get-namespace, que exibe informações sobre um namespace personalizado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ea641bab-e97b-4909-918e-447730027dc1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 58e84ec5c82ea3c2663b38bd2e53f65f2cf47519
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830879"
---
# <a name="get-namespace"></a>Get-namespace

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
|      /Namespace:<Namespace name>      | Especifica o nome do namespace. Observe que esse não é o nome amigável e deve ser exclusivo.<p>-Servidor de implantação: a sintaxe para o nome do namespace é/namspace: WDS:<ImageGroup>/<ImageName>/<Index>. Por exemplo: **WDS: ImageGroup1/install. wim/1**<br />-Servidor de transporte: esse valor deve corresponder ao nome fornecido para o namespace quando ele foi criado no servidor. |
|        [/Server:<Server name>]        |                                                                                                             Especifica o nome do servidor. Esse pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.                                                                                                              |
| [/Show: clients] ou [/details: clients] |                                                                                                                                                  Exibe informações sobre os computadores cliente que estão conectados ao namespace especificado.                                                                                                                                                  |

## <a name="examples"></a><a name=BKMK_examples></a>Disso
Para exibir informações sobre um namespace, digite:
```
wdsutil /Get-Namespace /Namespace:Custom Auto 1
```
Para exibir informações sobre um namespace e os clientes que estão conectados, digite um dos seguintes:
- Windows Server 2008: `wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1 /Show:Clients`
- Windows Server 2008 R2: `wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1 /details:Clients`
  ## <a name="additional-references"></a>Referências adicionais
  - A [chave de sintaxe de linha de comando](command-line-syntax-key.md)
  [usando o comando Get-mynamespaces](using-the-get-allnamespaces-command.md)
  [usando o comando New-namespace](using-the-new-namespace-command.md)
  [usando o comando Remove-namespace](using-the-remove-namespace-command.md)
  [subcomando: Start-namespace](subcommand-start-namespace.md)
