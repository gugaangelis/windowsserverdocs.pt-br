---
title: Delete-AutoaddDevices
description: Artigo de referência para Delete-AutoaddDevices, que exclui os computadores que estão pendentes, rejeitados ou aprovados do banco de dados de adição automática.
ms.topic: reference
ms.assetid: 8dcaca6a-212e-4c36-98e3-00938eef6b9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 50160d9b015861f2e32f7ae693cbd99d6eccb020
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038184"
---
# <a name="delete-autoadddevices"></a>Delete-AutoaddDevices

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exclui os computadores que estão pendentes, rejeitados ou aprovados do banco de dados de adição automática. Esse banco de dados armazena informações sobre esses computadores no servidor.

## <a name="syntax"></a>Sintaxe
```
wdsutil /delete-AutoaddDevices [/Server:<Server name>] /Devicetype:{PendingDevices | RejectedDevices |ApprovedDevices}
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|/DeviceType: {PendingDevices &#124; RejectedDevices &#124;ApprovedDevices}|Especifica o tipo de computador a ser excluído do banco de dados. Isso pode ser qualquer um dos três tipos a seguir:<p>-   **PendingDevices** retorna todos os computadores no banco de dados que têm um status pendente.<br />-   **RejectedDevices** retorna todos os computadores no banco de dados que têm o status rejeitado.<br />-   **ApprovedDevices** retorna todos os computadores que têm o status aprovado.|
## <a name="examples"></a>Exemplos
Para excluir todos os computadores rejeitados, digite:
```
wdsutil /delete-AutoaddDevices /Devicetype:RejectedDevices
```
Para excluir todos os computadores aprovados, digite:
```
wdsutil /verbose /delete-AutoaddDevices /Server:MyWDSServer /Devicetype:ApprovedDevices
```
## <a name="additional-references"></a>Referências adicionais
- Chave de sintaxe [de linha de comando](command-line-syntax-key.md) 
 [Usando o comando](using-the-approve-autoadddevices-command.md) 
 Approve-AutoaddDevices [Usando o comando](using-the-get-autoadddevices-command.md) 
 Get-AutoaddDevices [Usando o comando Reject-AutoaddDevices](using-the-reject-autoadddevices-command.md)
