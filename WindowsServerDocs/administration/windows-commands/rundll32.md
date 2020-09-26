---
title: rundll32
description: Artigo de referência para o comando rundll32, que carrega e executa bibliotecas de vínculo dinâmico (DLLs) de 32 bits.
ms.topic: reference
ms.assetid: 46d9cd64-8186-4cd4-a500-44700340fe81
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 95bcb4984c3488b2c00a588d972941b4eb840fdb
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91388410"
---
# <a name="rundll32"></a>rundll32

Carrega e executa bibliotecas de vínculo dinâmico (DLLs) de 32 bits. Não há configurações configuráveis para rundll32. As informações de ajuda são fornecidas para uma DLL específica que você executa com o comando **rundll32** .

Você deve executar o comando **rundll32** em um prompt de comandos com privilégios elevados. Para abrir um prompt de comando com privilégios elevados, clique em **Iniciar**, clique com o botão direito do mouse em **prompt de comando**e clique em **Executar como administrador**.

## <a name="syntax"></a>Sintaxe

```
rundll32 <DLLname>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| [Rundll32 printui.dll, PrintUIEntry](rundll32-printui.md) | Exibe a interface do usuário da impressora. |

## <a name="remarks"></a>Comentários

Rundll32 só pode chamar funções de uma DLL escrita explicitamente para ser chamada por rundll32.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
