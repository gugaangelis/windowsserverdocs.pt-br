---
title: Rejeitar-AutoaddDevices
description: Artigo de referência para Reject-AutoaddDevices, que rejeita computadores que estão com aprovação administrativa pendente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ea25a4b2-5fad-4360-9c47-c2c9df7ea31f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2b678f7a9fc875dfeebf735475db3adfb7ad9ae7
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85932425"
---
# <a name="reject-autoadddevices"></a>Rejeitar-AutoaddDevices

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
- Chave de sintaxe [de linha de comando](command-line-syntax-key.md) 
 [Usando o comando](using-the-approve-autoadddevices-command.md) 
 Approve-AutoaddDevices [Usando o comando](using-the-delete-autoadddevices-command.md) 
 delete-AutoaddDevices [Usando o comando Get-AutoaddDevices](using-the-get-autoadddevices-command.md)
