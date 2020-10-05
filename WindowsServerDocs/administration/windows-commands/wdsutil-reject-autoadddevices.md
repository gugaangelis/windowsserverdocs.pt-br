---
title: WDSUTIL Reject-AutoAddDevices
description: Artigo de referência para WDSUTIL Reject-AutoAddDevices, que rejeita computadores que estão com aprovação administrativa pendente.
ms.topic: reference
ms.assetid: ea25a4b2-5fad-4360-9c47-c2c9df7ea31f
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2cb970ce6b51933e72f208f6fbcc50bcb7b3164b
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729614"
---
# <a name="wdsutil-reject-autoadddevices"></a>WDSUTIL Reject-AutoAddDevices

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rejeita computadores que estão com aprovação administrativa pendente. Quando a política de adição automática está habilitada, a aprovação administrativa é necessária antes que computadores desconhecidos (aqueles que não são pré-configurados) possam instalar uma imagem. Você pode habilitar essa política usando a guia **resposta do PXE** da página de propriedades do servidor.
## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Reject-AutoaddDevices [/Server:<Server name>] /RequestId:<Request ID or ALL>
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|/RequestId: ID de solicitação de <&#124; todos os>|Especifica a ID da solicitação atribuída ao computador pendente. Para rejeitar todos os computadores pendentes, especifique **todos**.|
## <a name="examples"></a>Exemplos
Para rejeitar um único computador, digite:
```
wdsutil /Reject-AutoaddDevices /RequestId:12
```
Para rejeitar todos os computadores, digite:
```
wdsutil /verbose /Reject-AutoaddDevices /Server:MyWDSServer /RequestId:ALL
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [comando WDSUTIL Approve-AutoAddDevices](wdsutil-approve-autoadddevices.md)
- [comando WDSUTIL Delete-AutoAddDevices](wdsutil-delete-autoadddevices.md)
- [comando WDSUTIL Get-AutoAddDevices](wdsutil-get-autoadddevices.md)
