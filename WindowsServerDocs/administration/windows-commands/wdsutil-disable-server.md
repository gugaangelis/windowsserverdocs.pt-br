---
title: desabilitar-servidor
description: Artigo de referência para Disable-Server, que desabilita todos os serviços de um servidor de serviços de implantação do Windows.
ms.topic: reference
ms.assetid: b69fcfe0-b744-4794-bc75-2c9218c0ba66
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ed8fafb9c35f54656ab4551876dc0461ea1a2c55
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729689"
---
# <a name="disable-server"></a>desabilitar-servidor

Desabilita todos os serviços de um servidor de serviços de implantação do Windows.

## <a name="syntax"></a>Sintaxe

```
WDSUTIL [Options] /Disable-Server [/Server:<Server name>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[/Server:\<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|

## <a name="examples"></a>Exemplos

Para desabilitar o servidor, execute um dos seguintes:
```
WDSUTIL /Disable-Server
WDSUTIL /Verbose /Disable-Server /Server:MyWDSServer
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

