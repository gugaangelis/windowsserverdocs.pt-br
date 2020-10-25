---
title: desabilitar-servidor
description: Artigo de referência para Disable-Server, que desabilita todos os serviços de um servidor de serviços de implantação do Windows.
ms.topic: reference
ms.assetid: b69fcfe0-b744-4794-bc75-2c9218c0ba66
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6ff13092b5d99cd007d30eee80076f18814e138d
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524161"
---
# <a name="disable-server"></a>desabilitar-servidor

Desabilita todos os serviços de um servidor de serviços de implantação do Windows.

## <a name="syntax"></a>Sintaxe

```
wdsutil [Options] /Disable-Server [/Server:<Server name>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[/Server:\<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|

## <a name="examples"></a>Exemplos

Para desabilitar o servidor, execute um dos seguintes:
```
wdsutil /Disable-Server
wdsutil /Verbose /Disable-Server /Server:MyWDSServer
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

