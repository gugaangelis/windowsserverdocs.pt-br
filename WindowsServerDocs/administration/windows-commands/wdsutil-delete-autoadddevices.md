---
title: WDSUTIL Delete-AutoAddDevices
description: Artigo de referência para WDSUTIL Delete-AutoAddDevices, que exclui os computadores que estão pendentes, rejeitados ou aprovados do banco de dados de adição automática.
ms.topic: reference
ms.assetid: 8dcaca6a-212e-4c36-98e3-00938eef6b9c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 98c5aa38fd5394d4060cf9eba874d1be5cef845b
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729690"
---
# <a name="wdsutil-delete-autoadddevices"></a>WDSUTIL Delete-AutoAddDevices

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
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [comando WDSUTIL Approve-AutoAddDevices](wdsutil-approve-autoadddevices.md)
- [comando WDSUTIL Get-AutoAddDevices](wdsutil-get-autoadddevices.md)
- [comando WDSUTIL Reject-AutoAddDevices](wdsutil-reject-autoadddevices.md)
