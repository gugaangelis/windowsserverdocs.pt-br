---
title: rundll32
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 5b1f288d21a1dcac25ecc00f685ea179d8a6542f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835027"
---
# <a name="rundll32"></a>rundll32



Carrega e executa os 32 bits bibliotecas de vínculo dinâmico (DLLs). Não há nenhuma configuração configurável para Rundll32. Informações de ajuda são fornecidas para uma DLL específica, você pode executar com o **rundll32** comando.

Você deve executar o **rundll32** comando em um prompt de comando elevado. Para abrir um prompt de comando com privilégios elevados, clique em **inicie**, clique com botão direito **Prompt de comando**e, em seguida, clique em **executar como administrador**.

## <a name="syntax"></a>Sintaxe

```
Rundll32 <DLLname>
```

## <a name="commands"></a>Comandos

|Parâmetro|Descrição|
|---------|-----------|
|[rundll32 printui.dll,PrintUIEntry](rundll32-printui.md)|Exibe a interface do usuário de impressora|

## <a name="remarks"></a>Comentários

Rundll32 só pode chamar funções de uma DLL que são escritos explicitamente a ser chamado pelo Rundll32. Para obter mais informações sobre Rundll32 requisitos, consulte [164787 do artigo](https://go.microsoft.com/fwlink/?LinkID=165773) na Base de dados de Conhecimento Microsoft (https://go.microsoft.com/fwlink/?LinkID=165773).

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)