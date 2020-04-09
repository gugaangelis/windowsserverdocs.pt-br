---
title: obter namespaces
description: Tópico de comandos do Windows para Get-MyNamespaces, que exibe informações sobre todos os namespaces em um servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e8fe896d-a69a-4180-923b-9f18185f5941
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bbdba81f68f609c6ed7ba740b68b1ac1123913ab
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80831249"
---
# <a name="get-allnamespaces"></a>obter namespaces

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
### <a name="parameters"></a>Parâmetros

|         Parâmetro         |                                                                               Windows Server 2008                                                                               | Windows Server 2008 R2 |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
|  [/Server:<Server name>]  | Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado. |                        |
| [/ContentProvider:<name>] |                                                        Exibe somente os namespaces para o provedor de conteúdo especificado.                                                         |                        |
|      [/Show: clients]      |                            Com suporte apenas para o Windows Server 2008. Exibe informações sobre os computadores cliente que estão conectados ao namespace.                             |                        |
|    [/details: clientes]     |                           Com suporte apenas para o Windows Server 2008 R2. Exibe informações sobre os computadores cliente que estão conectados ao namespace.                           |                        |
|  [/ExcludedeletePending]  |                                                              Exclui todas as transmissões desativadas da lista.                                                              |                        |

## <a name="examples"></a><a name=BKMK_examples></a>Disso
Para exibir todos os namespaces, digite:
```
wdsutil /Get-AllNamespaces
```
Para exibir todos os namespaces, exceto aqueles que estão desativados, digite:
- Windows Server 2008
  ```
  wdsutil /Get-AllNamespaces /Server:MyWDSServer /ContentProvider:MyContentProv /Show:Clients /ExcludedeletePending
  ```
- Windows Server 2008 R2
  ```
  wdsutil /Get-AllNamespaces /Server:MyWDSServer /ContentProvider:MyContentProv /details:Clients /ExcludedeletePending
  ```
  ## <a name="additional-references"></a>Referências adicionais
  - [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
  [usando o comando new-namespace](using-the-new-namespace-command.md)
  [usando o comando Remove-namespace](using-the-remove-namespace-command.md)
  [subcomando: Start-namespace](subcommand-start-namespace.md)
