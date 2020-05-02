---
title: bitsadmin util e repairservice
description: Tópico de referência para o comando Bitsadmin util e repairservice, que corrige problemas conhecidos em várias versões do serviço BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ac7baeb-4340-4186-bfcb-66478195378d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0104a3f2ace972821151bf5083f9b0795e427ff1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82707650"
---
# <a name="bitsadmin-util-and-repairservice"></a>bitsadmin util e repairservice

Se o BITS não for iniciado, essa opção tentará resolver erros relacionados à configuração de serviço incorreta e às dependências nos serviços do Windows (como LANManworkstation) e no diretório de rede. Essa opção também gera saída que indica se os problemas foram resolvidos.

> [!NOTE]
> Esse comando não tem suporte no BITS 1,5 e versões anteriores.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /util /repairservice [/force]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| /Force | Opcional. Exclui e cria o serviço novamente.|

> [!NOTE]
> Se o BITS criar o serviço novamente, a cadeia de caracteres de descrição do serviço poderá ser definida como Inglês, mesmo em um sistema localizado.

## <a name="examples"></a>Exemplos

Para reparar a configuração do serviço BITS:

```
bitsadmin /util /repairservice
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin util](bitsadmin-util.md)

- [comando Bitsadmin](bitsadmin.md)
