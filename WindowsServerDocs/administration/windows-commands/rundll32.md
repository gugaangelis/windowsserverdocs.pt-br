---
title: rundll32
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 46d9cd64-8186-4cd4-a500-44700340fe81
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7345c1ad59a4209e607245db1b2a79055ffcb5fe
ms.sourcegitcommit: 1c75e4b3f5895f9fa33efffd06822dca301d4835
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77517282"
---
# <a name="rundll32"></a>rundll32



Carrega e executa bibliotecas de vínculo dinâmico (DLLs) de 32 bits. Não há configurações configuráveis para rundll32. As informações de ajuda são fornecidas para uma DLL específica que você executa com o comando **rundll32** .

Você deve executar o comando **rundll32** em um prompt de comandos com privilégios elevados. Para abrir um prompt de comando com privilégios elevados, clique em **Iniciar**, clique com o botão direito do mouse em **prompt de comando**e clique em **Executar como administrador**.

## <a name="syntax"></a>Sintaxe

```
Rundll32 <DLLname>
```

## <a name="commands"></a>Commands

|Parâmetro|Descrição|
|---------|-----------|
|[Rundll32 printui. dll, PrintUIEntry](rundll32-printui.md)|Exibe a interface do usuário da impressora|

## <a name="remarks"></a>Comentários

Rundll32 só pode chamar funções de uma DLL escrita explicitamente para ser chamada por rundll32.

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
