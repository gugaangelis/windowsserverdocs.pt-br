---
title: bitsadmin util e repairservice
description: Artigo de referência para o comando Bitsadmin util e repairservice, que corrige problemas conhecidos em várias versões do serviço BITS.
ms.topic: article
ms.assetid: 2ac7baeb-4340-4186-bfcb-66478195378d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4d90e6328376f52e60b598d8c2324b59877415db
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87880853"
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
