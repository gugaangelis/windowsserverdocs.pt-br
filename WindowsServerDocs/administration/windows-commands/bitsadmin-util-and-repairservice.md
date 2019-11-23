---
title: Bitsadmin util e repairservice
description: Tópico de comandos do Windows para **Bitsadmin util e repairservice** -Command usado para corrigir problemas conhecidos com várias versões do serviço bits.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 0ab06ac9c784cfa438eb285c28f0e661cf4b8302
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380282"
---
# <a name="bitsadmin-util-and-repairservice"></a>Bitsadmin util e repairservice

Se o BITS falhar ao iniciar, use essa opção para corrigir problemas conhecidos com várias versões do BITS.

**BITSAdmin 1,5 e anterior:** não há suporte para .

## <a name="syntax"></a>Sintaxe

```
bitsadmin /Util /RepairService [/Force]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Force|Opcional — exclui e recria o serviço.|

## <a name="remarks"></a>Comentários

Essa opção resolve erros relacionados à configuração de serviço incorreta e dependências nos serviços do Windows (como LANManworkstation) e no diretório de rede. Essa opção gera saída que indica se os problemas foram resolvidos.

> [!NOTE]
> Se o BITS recriar o serviço, a cadeia de caracteres de descrição do serviço poderá ser definida como Inglês em um sistema localizado.

> [!IMPORTANT]
> Não há suporte para esse comando no Windows Vista.

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir repara a configuração do serviço BITS.
```
C:\>bitsadmin /Util /RepairService
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)