---
title: rundll32
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 46d9cd64-8186-4cd4-a500-44700340fe81
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 391607f6e744fe88a60cb556067cf2699eee25f5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835429"
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

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
