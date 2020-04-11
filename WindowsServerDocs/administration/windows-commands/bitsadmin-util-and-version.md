---
title: utilitário e versão do Bitsadmin
description: Tópico de comandos do Windows para o **Bitsadmin util e Version**, que exibe a versão do serviço bits.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 98f17328-dfbd-4cbb-93c1-b8d424bc3f0a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2c2518eb7a8f15d9a592ed9a77dd67a6f8d8afac
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122467"
---
# <a name="bitsadmin-util-and-version"></a>utilitário e versão do Bitsadmin

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

O exemplo a seguir a versão do serviço BITS.

```
C:\>bitsadmin /util /version
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)