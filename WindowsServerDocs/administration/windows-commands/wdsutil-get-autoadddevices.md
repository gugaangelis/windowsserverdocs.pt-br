---
title: WDSUTIL Get-AutoAddDevices
description: Artigo de referência para o WDSUTIL Get-AutoAddDevices, que exibe todos os computadores que estão no banco de dados de adição automática em um servidor dos serviços de implantação do Windows.
ms.topic: reference
ms.assetid: 24b4b688-55b0-4bd9-a2f5-7ef4b3dfe2f2
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c7d5be85ce29a41714d70d8bdfa11e3afa1661d6
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729655"
---
# <a name="wdsutil-get-autoadddevices"></a>WDSUTIL Get-AutoAddDevices

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
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [comando WDSUTIL Delete-AutoAddDevices](wdsutil-delete-autoadddevices.md)
- [comando WDSUTIL Approve-AutoAddDevices](wdsutil-approve-autoadddevices.md)
- [comando WDSUTIL Reject-AutoAddDevices](wdsutil-reject-autoadddevices.md)
