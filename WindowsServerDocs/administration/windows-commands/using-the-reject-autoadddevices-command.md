---
title: Usando o comando AutoaddDevices de rejeição
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: af46aec7c8f02b3600983b66bd1b0ac6f5dd1dcc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852557"
---
# <a name="using-the-reject-autoadddevices-command"></a>Usando o comando AutoaddDevices de rejeição

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rejeita os computadores que estão aguardando aprovação administrativa. Quando a política de adição automática está habilitada, é necessária aprovação administrativa antes que os computadores desconhecidos (aquelas que não foram pré-testados) possam instalar uma imagem. Você pode habilitar essa política usando o **resposta do PXE** guia da página de propriedades de servidor s.
## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Reject-AutoaddDevices [/Server:<Server name>] /RequestId:<Request ID or ALL>
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
|/RequestId:<Request ID &#124; ALL>|Especifica a ID da solicitação atribuída ao computador pendente. Para rejeitar computadores pendentes, especifique **todos os**.|
## <a name="BKMK_examples"></a>Exemplos
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
[usando o comando aprovar AutoaddDevices](using-the-approve-autoadddevices-command.md)
[usando o comando delete AutoaddDevices](using-the-delete-autoadddevices-command.md) 
 [ Usando o comando get-AutoaddDevices](using-the-get-autoadddevices-command.md)
