---
title: WDSUTIL Delete-AutoAddDevices
description: Artigo de referência para o comando WDSUTIL Delete-AutoAddDevices, que exclui os computadores que estão pendentes, rejeitados ou aprovados do banco de dados de adição automática.
ms.topic: reference
ms.assetid: 8dcaca6a-212e-4c36-98e3-00938eef6b9c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 610b57c7db49c5d8cf354502634cf82212d52c19
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524811"
---
# <a name="wdsutil-delete-autoadddevices"></a>WDSUTIL Delete-AutoAddDevices

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exclui os computadores que estão pendentes, rejeitados ou aprovados do banco de dados de adição automática. Esse banco de dados armazena informações sobre esses computadores no servidor.

## <a name="syntax"></a>Sintaxe

```
wdsutil /delete-AutoaddDevices [/Server:<Servername>] /Devicetype:{PendingDevices | RejectedDevices |ApprovedDevices}
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| [/Server:`<Servername>`] | Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado. |
| DeviceType`{PendingDevices|RejectedDevices|ApprovedDevices}` | Especifica o tipo de computador a ser excluído do banco de dados. Esse tipo pode ser **PendingDevices**, que retorna todos os computadores no banco de dados que têm um status de pendente, **RejectedDevices**, que retorna todos os computadores no banco de dados que têm um status de rejeitado ou **ApprovedDevices**, que retorna todos os computadores com o status aprovado. |

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

- [Cmdlets dos serviços de implantação do Windows](/powershell/module/wds)
