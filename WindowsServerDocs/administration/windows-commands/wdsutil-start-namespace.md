---
title: WDSUTIL Start-namespace
description: Artigo de referência para o subcomando Start-namespace, que inicia um namespace de conversão agendada.
ms.topic: reference
ms.assetid: 2dd1c11e-6ab7-4129-9e3a-3f80e0ba59c0
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7463b062bf3322d6cf4f9c481effde825cb2979a
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729824"
---
# <a name="wdsutil-start-namespace"></a>WDSUTIL Start-namespace

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Inicia um namespace de conversão agendada.

## <a name="syntax"></a>Sintaxe
```
wdsutil /start-Namespace /Namespace:<Namespace name[/Server:<Server name>]
```
### <a name="parameters"></a>Parâmetros

|          Parâmetro          |                                                                                                                                                                                             Descrição                                                                                                                                                                                             |
|-----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /Namespace: nome do namespace de <| Especifica o nome do namespace. Observe que esse não é o nome amigável e deve ser exclusivo.<p>-   **Servidor de implantação**: a sintaxe para o nome do namespace é/NAMSPACE: WDS: <Image group> / <Image name> / <Index> . Por exemplo: **WDS: ImageGroup1/install. wim/1**<br />-   **Servidor de transporte**: esse nome deve corresponder ao nome fornecido para o namespace quando ele foi criado no servidor. |
|   [/Server:<Server name>]   |                                                                                                           Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.                                                                                                           |

## <a name="examples"></a>Exemplos
Para iniciar um namespace, digite um dos seguintes:
```
wdsutil /start-Namespace /Namespace:Custom Auto 1
wdsutil /start-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [comando WDSUTIL Get-MyNamespaces](wdsutil-get-allnamespaces.md)
- [comando WDSUTIL New-namespace](wdsutil-new-namespace.md)
- [comando WDSUTIL Remove-namespace](wdsutil-remove-namespace.md)
