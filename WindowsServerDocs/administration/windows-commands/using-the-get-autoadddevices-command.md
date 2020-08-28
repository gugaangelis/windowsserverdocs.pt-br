---
title: Get-AutoaddDevices
description: Artigo de referência para Get-AutoaddDevices, que exibe todos os computadores que estão no banco de dados de adição automática em um servidor dos serviços de implantação do Windows.
ms.topic: reference
ms.assetid: 24b4b688-55b0-4bd9-a2f5-7ef4b3dfe2f2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f449ddb58bb4e5f28a3cee02e9c769d363baf3b7
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89035894"
---
# <a name="get-autoadddevices"></a>Get-AutoaddDevices

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe todos os computadores que estão no banco de dados de adição automática em um servidor dos serviços de implantação do Windows.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Get-AutoaddDevices [/Server:<Server name>] /Devicetype:{PendingDevices | RejectedDevices | ApprovedDevices}
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|/DeviceType: {PendingDevices &#124; RejectedDevices &#124; ApprovedDevices}|Especifica o tipo de computador a ser retornado.<p>-   **PendingDevices** retorna todos os computadores no banco de dados que têm um status pendente.<br />-   **RejectedDevices** retorna todos os computadores no banco de dados que têm o status rejeitado.<br />-   **ApprovedDevices** retorna todos os computadores no banco de dados que têm o status aprovado.|
## <a name="examples"></a>Exemplos
Para ver todos os computadores aprovados, digite:
```
wdsutil /Get-AutoaddDevices /Devicetype:ApprovedDevices
```
Para ver todos os computadores rejeitados, digite:
```
wdsutil /verbose /Get-AutoaddDevices /Devicetype:RejectedDevices /Server:MyWDSServer
```
## <a name="additional-references"></a>Referências adicionais
- Chave de sintaxe [de linha de comando](command-line-syntax-key.md) 
 [Usando o comando](using-the-delete-autoadddevices-command.md) 
 delete-AutoaddDevices [Usando o comando](using-the-approve-autoadddevices-command.md) 
 Approve-AutoaddDevices [Usando o comando Reject-AutoaddDevices](using-the-reject-autoadddevices-command.md)
