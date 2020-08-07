---
title: ksetup setcomputerpassword
description: Artigo de referência para o comando ksetup setcomputerpassword, que define a senha para o computador local.
ms.topic: article
ms.assetid: e307d8f6-3b93-4c24-ac04-f31549f7dc7d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8d9dba34490616b07671ada16e0c76f0122c3d6a
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87887770"
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
