---
title: WDSUTIL habilitar-servidor
description: Artigo de referência para WDSUTIL Enable-Server, que habilita todos os serviços para serviços de implantação do Windows.
ms.topic: reference
ms.assetid: 939ffbfb-cf3c-4310-9627-6e7e0c0644d6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 3ec32f2bd0d269795e1f88b967804c2bb82ac9f4
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729678"
---
# <a name="wdsutil-enable-server"></a>WDSUTIL habilitar-servidor

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Habilita todos os serviços para serviços de implantação do Windows.

## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Enable-Server [/Server:<Server name>]
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|
## <a name="examples"></a>Exemplos
Para habilitar os serviços no servidor, execute um dos seguintes procedimentos:
```
wdsutil /Enable-Server
wdsutil /verbose /Enable-Server /Server:MyWDSServer
```
## <a name="additional-references"></a>Referências adicionais
- Chave de sintaxe [de linha de comando](command-line-syntax-key.md) 
 [WDSUTIL Disable – comando](wdsutil-disable-server.md) 
 do servidor comando WDSUTIL Get [-Server](wdsutil-get-server.md) 
 comando do servidor [de inicialização WDSUTIL](wdsutil-initialize-server.md) 
 [WDSUTIL Set-Server Command](wdsutil-set-server.md) 
 comando Start- [Server do WDSUTIL](wdsutil-start-server.md) 
 comando WDSUTIL Stop [-Server](wdsutil-stop-server.md) 
 [WDSUTIL não inicializar-comando de servidor](wdsutil-uninitialize-server.md)
