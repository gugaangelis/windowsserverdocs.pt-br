---
title: Bitsadmin util e repairservice
description: Tópico de comandos do Windows para Bitsadmin util e repairservice, que corrige problemas conhecidos em várias versões do serviço BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ac7baeb-4340-4186-bfcb-66478195378d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aaaa6edab22031dc53d266984bb669634e3bb362
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848889"
---
# <a name="bitsadmin-util-and-repairservice"></a>Bitsadmin util e repairservice

Se o BITS falhar ao iniciar, use essa opção para corrigir problemas conhecidos em várias versões do BITS.

**BITSAdmin 1,5 e anterior:** não há suporte para .

## <a name="syntax"></a>Sintaxe

```
bitsadmin /Util /RepairService [/Force]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Force|Opcional — exclui e recria o serviço.|

## <a name="remarks"></a>Comentários

Essa opção resolve erros relacionados à configuração de serviço incorreta e dependências nos serviços do Windows (como LANManworkstation) e no diretório de rede. Essa opção gera saída que indica se os problemas foram resolvidos.

> [!NOTE]
> Se o BITS recriar o serviço, a cadeia de caracteres de descrição do serviço poderá ser definida como Inglês em um sistema localizado.

> [!IMPORTANT]
> Não há suporte para esse comando no Windows Vista.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir repara a configuração do serviço BITS.
```
C:\>bitsadmin /Util /RepairService
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)