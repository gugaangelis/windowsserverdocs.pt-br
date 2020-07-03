---
title: bitsadmin util e version
description: Artigo de referência para o Bitsadmin util e o comando Version, que exibe a versão do serviço BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 98f17328-dfbd-4cbb-93c1-b8d424bc3f0a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9b0a1a6b6c866acafa8eaccd6ade170abd58bf01
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927292"
---
# <a name="bitsadmin-util-and-version"></a>bitsadmin util e version

Exibe a versão do serviço BITS (por exemplo, 2,0).

> [!NOTE]
> Esse comando não tem suporte no BITS 1,5 e versões anteriores.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /util /version [/verbose]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| /verbose | Use essa opção para exibir a versão do arquivo para cada DLL relacionada ao BITS e para verificar se o serviço BITS pode ser iniciado.|

## <a name="examples"></a>Exemplos

Para exibir a versão do serviço BITS.

```
bitsadmin /util /version
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin util](bitsadmin-util.md)

- [comando Bitsadmin](bitsadmin.md)
