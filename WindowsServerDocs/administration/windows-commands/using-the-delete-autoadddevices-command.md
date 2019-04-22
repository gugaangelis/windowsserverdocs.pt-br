---
title: Usando o comando delete AutoaddDevices
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8dcaca6a-212e-4c36-98e3-00938eef6b9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8b3375418c5ce0b02e187e292cac5b168f0de5dc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813497"
---
# <a name="using-the-delete-autoadddevices-command"></a>Usando o comando delete AutoaddDevices

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exclui os computadores que estão pendentes, rejeitados ou aprovada do banco de dados de adição automática. Este banco de dados armazena informações sobre esses computadores no servidor.
## <a name="syntax"></a>Sintaxe
```
wdsutil /delete-AutoaddDevices [/Server:<Server name>] /Devicetype:{PendingDevices | RejectedDevices |ApprovedDevices}
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|/ Devicetype: {PendingDevices &#124; RejectedDevices &#124;ApprovedDevices}|Especifica o tipo de computador para excluir do banco de dados. Isso pode ser qualquer um dos três tipos:<br /><br />-   **PendingDevices** retorna todos os computadores no banco de dados que têm um status pendente.<br />-   **RejectedDevices** retorna todos os computadores no banco de dados que têm um status de rejeitada.<br />-   **ApprovedDevices** retorna todos os computadores que têm um status de aprovação.|
## <a name="BKMK_examples"></a>Exemplos
Para excluir computadores rejeitados tudo, digite:
```
wdsutil /delete-AutoaddDevices /Devicetype:RejectedDevices
```
Para excluir todos os aprovados de computadores, digite:
```
wdsutil /verbose /delete-AutoaddDevices /Server:MyWDSServer /Devicetype:ApprovedDevices
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando aprovar AutoaddDevices](using-the-approve-autoadddevices-command.md)
[usando o comando get-AutoaddDevices](using-the-get-autoadddevices-command.md) 
 [Usando o comando AutoaddDevices de rejeição](using-the-reject-autoadddevices-command.md)
