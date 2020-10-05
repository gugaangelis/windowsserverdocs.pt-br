---
title: WDSUTIL Start-Server
description: Artigo de referência para o subcomando Start-Server, que inicia todos os serviços para um servidor de serviços de implantação do Windows.
ms.topic: reference
ms.assetid: 1e4343e2-0a16-4e65-8769-c09adaef5680
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 03791bd3a313f1ab2a5a2e076036d0aaddb18a8e
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729840"
---
# <a name="wdsutil-start-server"></a>WDSUTIL Start-Server

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Inicia todos os serviços para um servidor de serviços de implantação do Windows.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /start-Server [/Server:<Server name>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor a ser iniciado. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
## <a name="examples"></a>Exemplos
Para iniciar o servidor, digite um dos seguintes:
```
wdsutil /start-Server
wdsutil /verbose /start-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [WDSUTIL Disable – comando do servidor](wdsutil-disable-server.md)
- [WDSUTIL Enable-Server comando](wdsutil-enable-server.md)
- [comando WDSUTIL Get-Server](wdsutil-get-server.md)
- [comando do servidor de inicialização WDSUTIL](wdsutil-initialize-server.md)
- [WDSUTIL Set-Server Command](wdsutil-set-server.md)
- [comando WDSUTIL Stop-Server](wdsutil-stop-server.md)
- [comando Start-Server do WDSUTIL](wdsutil-start-server.md)
- [WDSUTIL não inicializar-comando de servidor](wdsutil-uninitialize-server.md)
