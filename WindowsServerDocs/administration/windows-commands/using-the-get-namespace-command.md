---
title: Usando o comando get-Namespace
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ea641bab-e97b-4909-918e-447730027dc1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8c30f9ef375bdf368f81f5a69961746851a2aac8
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440436"
---
# <a name="using-the-get-namespace-command"></a>Usando o comando get-Namespace

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
## <a name="parameters"></a>Parâmetros

|               Parâmetro               |                                                                                                                                                                                         Descrição                                                                                                                                                                                          |
|---------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      /Namespace:<Namespace name>      | Especifica o nome do namespace. Observe que isso não é o nome amigável, e ele deve ser exclusivo.<br /><br />-Implantação servidor: A sintaxe de nome de namespace é /Namspace:WDS:<ImageGroup>/<ImageName>/<Index>. Por exemplo:  **WDS:ImageGroup1/install.wim/1**<br />-Servidor de transporte: Esse valor deve corresponder ao nome fornecido para o namespace quando ele foi criado no servidor. |
|        [/Server:<Server name>]        |                                                                                                             Especifica o nome do servidor. Isso pode ser o nome NetBIOS ou o nome de domínio totalmente qualificado (FQDN). Se nenhum nome de servidor for especificado, o servidor local será usado.                                                                                                              |
| [/ Mostrar: os clientes] ou [/ detalhes: clientes] |                                                                                                                                                  Exibe informações sobre computadores cliente que estão conectados ao namespace especificado.                                                                                                                                                  |

## <a name="BKMK_examples"></a>Exemplos
Para exibir informações sobre um namespace, digite:
```
wdsutil /Get-Namespace /Namespace:"Custom Auto 1"
```
Para exibir informações sobre um namespace e os clientes que estão conectados, digite um dos seguintes:
- Windows Server 2008: `wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1" /Show:Clients`
- Windows Server 2008 R2: `wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1" /details:Clients`
  #### <a name="additional-references"></a>Referências adicionais
  [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
  [usando o comando get-AllNamespaces](using-the-get-allnamespaces-command.md)
  [usando o comando Novo Namespace](using-the-new-namespace-command.md)
  [Using o comando remove-Namespace](using-the-remove-namespace-command.md)
  [subcomando: Namespace de início](subcommand-start-namespace.md)
