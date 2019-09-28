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
ms.openlocfilehash: 29a87f9f07c25a0c671e47550e0a054d8308f747
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384422"
---
# <a name="rundll32"></a>rundll32



Carrega e executa bibliotecas de vínculo dinâmico (DLLs) de 32 bits. Não há configurações configuráveis para rundll32. As informações de ajuda são fornecidas para uma DLL específica que você executa com o comando **rundll32** .

Você deve executar o comando **rundll32** em um prompt de comandos com privilégios elevados. Para abrir um prompt de comando com privilégios elevados, clique em **Iniciar**, clique com o botão direito do mouse em **prompt de comando**e clique em **Executar como administrador**.

## <a name="syntax"></a>Sintaxe

```
Rundll32 <DLLname>
```

## <a name="commands"></a>Comandos

|Parâmetro|Descrição|
|---------|-----------|
|[Rundll32 printui. dll, PrintUIEntry](rundll32-printui.md)|Exibe a interface do usuário da impressora|

## <a name="remarks"></a>Comentários

Rundll32 só pode chamar funções de uma DLL que são explicitamente gravadas para serem chamadas por rundll32. Para obter mais informações sobre os requisitos de rundll32, consulte o [artigo 164787](https://go.microsoft.com/fwlink/?LinkID=165773) na base de dados de conhecimento Microsoft (https://go.microsoft.com/fwlink/?LinkID=165773).

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)