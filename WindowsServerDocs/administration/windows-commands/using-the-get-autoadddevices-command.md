---
title: Usando o comando Get-AutoaddDevices
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 24b4b688-55b0-4bd9-a2f5-7ef4b3dfe2f2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fa30a8ebd73164dc3d8ab267c3deb0739aa4b700
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363253"
---
# <a name="using-the-get-autoadddevices-command"></a>Usando o comando Get-AutoaddDevices

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe todos os computadores que estão no banco de dados de adição automática em um servidor dos serviços de implantação do Windows.
## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Get-AutoaddDevices [/Server:<Server name>] /Devicetype:{PendingDevices | RejectedDevices | ApprovedDevices}
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|/DeviceType: {PendingDevices &#124; RejectedDevices &#124; ApprovedDevices}|Especifica o tipo de computador a ser retornado.<br /><br />-   **PendingDevices** retorna todos os computadores no banco de dados que têm um status pendente.<br />-   **RejectedDevices** retorna todos os computadores no banco de dados que têm o status rejeitado.<br />-   **ApprovedDevices** retorna todos os computadores no banco de dados que têm o status aprovado.|
## <a name="BKMK_examples"></a>Disso
Para ver todos os computadores aprovados, digite:
```
wdsutil /Get-AutoaddDevices /Devicetype:ApprovedDevices
```
Para ver todos os computadores rejeitados, digite:
```
wdsutil /verbose /Get-AutoaddDevices /Devicetype:RejectedDevices /Server:MyWDSServer
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando delete-AutoaddDevices](using-the-delete-autoadddevices-command.md)
[usando o comando Approve-AutoaddDevices](using-the-approve-autoadddevices-command.md)
[usando o comando Reject-AutoaddDevices](using-the-reject-autoadddevices-command.md)
