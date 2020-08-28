---
title: ksetup setcomputerpassword
description: Artigo de referência para o comando ksetup setcomputerpassword, que define a senha para o computador local.
ms.topic: reference
ms.assetid: e307d8f6-3b93-4c24-ac04-f31549f7dc7d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bdddfa424b1f34e084c9e03cb441759a64b903c1
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025370"
---
# <a name="ksetup-setcomputerpassword"></a>ksetup setcomputerpassword

Define a senha para o computador local. Esse comando afeta apenas a conta do computador e requer uma reinicialização para que a alteração da senha entre em vigor.

> [!IMPORTANT]
> A senha da conta de computador não é exibida no registro ou como saída do comando **ksetup** .

## <a name="syntax"></a>Sintaxe

```
ksetup /setcomputerpassword <password>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<password>` | Especifica a senha fornecida para definir a conta de computador no computador local. A senha só pode ser definida usando uma conta com privilégios administrativos e a senha deve ter de 1 a 156 caracteres alfanuméricos ou especiais. |

### <a name="examples"></a>Exemplos

Para alterar a senha da conta de computador no computador local de *IPops897* para *IPop $897!*, digite:

```
ksetup /setcomputerpassword IPop$897!
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando ksetup](ksetup.md)
