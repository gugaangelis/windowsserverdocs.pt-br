---
title: Início do subcomando-namespace
description: Artigo de referência para o subcomando Start-namespace, que inicia um namespace de conversão agendada.
ms.topic: reference
ms.assetid: 2dd1c11e-6ab7-4129-9e3a-3f80e0ba59c0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 38a0dd1b4988977d14a2be68966a6eb53df71ce8
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024750"
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
| /Namespace: nome do namespace de <| Especifica o nome do namespace. Observe que esse não é o nome amigável e deve ser exclusivo.<p>-   **Servidor de implantação**: a sintaxe para o nome do namespace é/NAMSPACE: WDS: <Image group> / <Image name> / <Index> . Por exemplo: **WDS: ImageGroup1/install. wim/1**<br />-   **Servidor de transporte**: esse nome deve corresponder ao nome fornecido para o namespace quando ele foi criado no servidor. |
|   [/Server:<Server name>]   |                                                                                                           Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.                                                                                                           |

## <a name="examples"></a>Exemplos
Para iniciar um namespace, digite um dos seguintes:
```
wdsutil /start-Namespace /Namespace:Custom Auto 1
wdsutil /start-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1
```
## <a name="additional-references"></a>Referências adicionais
- Chave de sintaxe [de linha de comando](command-line-syntax-key.md) 
 [Usando o comando](using-the-get-allnamespaces-command.md) 
 Get-MyNamespaces [Usando o comando](using-the-new-namespace-command.md) 
 New-namespace [Usando o comando Remove-namespace](using-the-remove-namespace-command.md)
