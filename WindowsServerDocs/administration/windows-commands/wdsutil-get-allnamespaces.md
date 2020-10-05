---
title: WDSUTIL Get-objectnamespaces
description: Artigo de referência para WDSUTIL Get-MyNamespaces, que exibe informações sobre todos os namespaces em um servidor.
ms.topic: reference
ms.assetid: e8fe896d-a69a-4180-923b-9f18185f5941
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ddad534e46c4ca30311ad497a9fa66d60a4cda60
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729657"
---
# <a name="wdsutil-get-allnamespaces"></a>WDSUTIL Get-objectnamespaces

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe informações sobre todos os namespaces em um servidor.

## <a name="syntax"></a>Syntax
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
| [/ContentProvider: <name> ] |                                                        Exibe somente os namespaces para o provedor de conteúdo especificado.                                                         |                        |
|      [/Show: clients]      |                            Com suporte apenas para o Windows Server 2008. Exibe informações sobre os computadores cliente que estão conectados ao namespace.                             |                        |
|    [/details: clientes]     |                           Com suporte apenas para o Windows Server 2008 R2. Exibe informações sobre os computadores cliente que estão conectados ao namespace.                           |                        |
|  [/ExcludedeletePending]  |                                                              Exclui todas as transmissões desativadas da lista.                                                              |                        |

## <a name="examples"></a>Exemplos
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
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [comando WDSUTIL New-namespace](wdsutil-new-namespace.md)
- [comando WDSUTIL Remove-namespace](wdsutil-remove-namespace.md)
- [comando WDSUTIL Start-nmespace](wdsutil-start-namespace.md)
