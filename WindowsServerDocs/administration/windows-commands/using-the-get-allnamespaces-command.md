---
title: Usando o comando Get-MyNamespaces
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e8fe896d-a69a-4180-923b-9f18185f5941
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0cd90fc650271c863459dd809e47ca6309132de5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363293"
---
# <a name="using-the-get-allnamespaces-command"></a>Usando o comando Get-MyNamespaces

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe informações sobre todos os namespaces em um servidor.
## <a name="syntax"></a>Sintaxe
Windows Server 2008:
```
wdsutil /Get-AllNamespaces [/Server:<Server name>] [/ContentProvider:<name>] [/Show:Clients] [/ExcludedeletePending]
```
Windows Server 2008 R2:
```
wdsutil /Get-AllNamespaces [/Server:<Server name>] [/ContentProvider:<name>] [/details:Clients] [/ExcludedeletePending]
```
## <a name="parameters"></a>Parâmetros

|         Parâmetro         |                                                                               Windows Server 2008                                                                               | Windows Server 2008 R2 |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
|  [/Server:<Server name>]  | Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado. |                        |
| [/ContentProvider: <name>] |                                                        Exibe somente os namespaces para o provedor de conteúdo especificado.                                                         |                        |
|      [/Show: clients]      |                            Com suporte apenas para o Windows Server 2008. Exibe informações sobre os computadores cliente que estão conectados ao namespace.                             |                        |
|    [/details: clientes]     |                           Com suporte apenas para o Windows Server 2008 R2. Exibe informações sobre os computadores cliente que estão conectados ao namespace.                           |                        |
|  [/ExcludedeletePending]  |                                                              Exclui todas as transmissões desativadas da lista.                                                              |                        |

## <a name="BKMK_examples"></a>Disso
Para exibir todos os namespaces, digite:
```
wdsutil /Get-AllNamespaces
```
Para exibir todos os namespaces, exceto aqueles que estão desativados, digite:
- Windows Server 2008
  ```
  wdsutil /Get-AllNamespaces /Server:MyWDSServer /ContentProvider:"MyContentProv" /Show:Clients /ExcludedeletePending
  ```
- Windows Server 2008 R2
  ```
  wdsutil /Get-AllNamespaces /Server:MyWDSServer /ContentProvider:"MyContentProv" /details:Clients /ExcludedeletePending
  ```
  #### <a name="additional-references"></a>Referências adicionais
  [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
  [usando o comando new-namespace](using-the-new-namespace-command.md)
  [usando o comando Remove-namespace](using-the-remove-namespace-command.md)
  [subcomando: Start-namespace](subcommand-start-namespace.md)
