---
title: WDSUTIL Get-namespace
description: Artigo de referência para WDSUTIL Get-namespace, que exibe informações sobre um namespace personalizado.
ms.topic: reference
ms.assetid: ea641bab-e97b-4909-918e-447730027dc1
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2c88bd26af900950c33b059822c08f5afb40de77
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729637"
---
# <a name="wdsutil-get-namespace"></a>WDSUTIL Get-namespace

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe informações sobre um namespace personalizado.

## <a name="syntax"></a>Syntax
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
- Windows Server 2008: `wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1 /Show:Clients`
- Windows Server 2008 R2: `wdsutil /Get-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1 /details:Clients`

## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [comando WDSUTIL Get-MyNamespaces](wdsutil-get-allnamespaces.md)
- [comando WDSUTIL New-namespace](wdsutil-new-namespace.md)
- [comando WDSUTIL Remove-namespace](wdsutil-remove-namespace.md)
- [comando Start-namespace WDSUTIL](wdsutil-start-namespace.md)
