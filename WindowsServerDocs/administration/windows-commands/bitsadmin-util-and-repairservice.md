---
title: REPAIRSERVICE e bitsadmin util
description: Tópico de comandos do Windows para **util bitsadmin e repairservice** -comando usado para corrigir os problemas conhecidos com várias versões do serviço do BITS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2ac7baeb-4340-4186-bfcb-66478195378d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bc5101378a389c865f5753146b711be0d15c6785
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852087"
---
# <a name="bitsadmin-util-and-repairservice"></a>REPAIRSERVICE e bitsadmin util

Se o BITS não for iniciado, use essa opção para corrigir os problemas conhecidos com várias versões de BITS.

**BITSAdmin 1.5 e anterior:** não tem suporte.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /Util /RepairService [/Force]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Force|Opcional – exclui e recria o serviço.|

## <a name="remarks"></a>Comentários

Essa opção elimina a configuração de serviço relacionado ao incorreto de erros e dependências em serviços do Windows (como LANManworkstation) e o diretório de rede. Essa opção gera a saída que indica se os problemas que foram resolvidos.

> [!NOTE]
> Se o BITS recria o serviço, a cadeia de caracteres de descrição de serviço pode ser definida para inglês em um sistema localizado.

> [!IMPORTANT]
> Este comando não é suportado no Windows Vista.

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir repara a configuração de serviço do BITS.
```
C:\>bitsadmin /Util /RepairService
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)