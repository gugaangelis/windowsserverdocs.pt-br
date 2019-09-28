---
title: Usando o comando Reject-AutoaddDevices
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ea25a4b2-5fad-4360-9c47-c2c9df7ea31f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2e8fda3037ef921e2b2a7a0acb616b8a67545ff9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362996"
---
# <a name="using-the-reject-autoadddevices-command"></a>Usando o comando Reject-AutoaddDevices

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rejeita computadores que estão com aprovação administrativa pendente. Quando a política de adição automática está habilitada, a aprovação administrativa é necessária antes que computadores desconhecidos (aqueles que não são pré-configurados) possam instalar uma imagem. Você pode habilitar essa política usando a guia **resposta do PXE** da página de propriedades do servidor.
## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Reject-AutoaddDevices [/Server:<Server name>] /RequestId:<Request ID or ALL>
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|/RequestId: todos os > &#124; ID da solicitação de <|Especifica a ID da solicitação atribuída ao computador pendente. Para rejeitar todos os computadores pendentes, especifique **todos**.|
## <a name="BKMK_examples"></a>Disso
Para rejeitar um único computador, digite:
```
wdsutil /Reject-AutoaddDevices /RequestId:12
```
Para rejeitar todos os computadores, digite:
```
wdsutil /verbose /Reject-AutoaddDevices /Server:MyWDSServer /RequestId:ALL
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando Approve-AutoaddDevices](using-the-approve-autoadddevices-command.md)
[usando o comando delete-AutoaddDevices](using-the-delete-autoadddevices-command.md)
[usando o comando Get-AutoaddDevices](using-the-get-autoadddevices-command.md)
