---
title: Subcomando start-Namespace
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2dd1c11e-6ab7-4129-9e3a-3f80e0ba59c0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5dc484a3b159cd510d7a61484f4f46b1f1db7a4e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877127"
---
# <a name="subcommand-start-namespace"></a>Subcommand: start-Namespace

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 inicia um namespace de multicast programado.
## <a name="syntax"></a>Sintaxe
```
wdsutil /start-Namespace /Namespace:<Namespace name> [/Server:<Server name>]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/Namespace:<Namespace name>|Especifica o nome do namespace. Observe que isso não é o nome amigável, e ele deve ser exclusivo.<br /><br />-   **Servidor de implantação**: A sintaxe de nome de namespace é /Namspace:WDS:<Image group>/<Image name>/<Index>. Por exemplo:  **WDS:ImageGroup1/install.wim/1**<br />-   **Servidor de transporte**: Esse nome deve corresponder ao nome fornecido para o namespace quando ele foi criado no servidor.|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
## <a name="BKMK_examples"></a>Exemplos
Para iniciar um namespace, digite um dos seguintes:
```
wdsutil /start-Namespace /Namespace:"Custom Auto 1"
wdsutil /start-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1"
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando get-AllNamespaces](using-the-get-allnamespaces-command.md)
[usando o comando Novo Namespace](using-the-new-namespace-command.md)
[Using o comando remove-Namespace](using-the-remove-namespace-command.md)
