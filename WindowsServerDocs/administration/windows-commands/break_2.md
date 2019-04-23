---
title: break
description: Tópico de comandos do Windows para **break_2** - desassocia um volume de cópia de sombra do VSS e o torna acessível como um volume normal.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: de2b6c95-1c2e-4a43-bec5-341a9014371b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 49516539ae603e2c93b3fc395c77786be790d663
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886327"
---
# <a name="break"></a>break



Desassocia um volume de cópia de sombra do VSS e o torna acessível como um volume normal. O volume pode ser acessado usando uma letra da unidade (se atribuído) ou o nome do volume. Se usado sem parâmetros, **quebra** exibe a Ajuda no prompt de comando.

> [!NOTE]
> Esse comando é relevante apenas para cópias de sombra de hardware após a importação.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
break [writable] <SetID>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[gravável]|Permite leitura/gravação acesso no volume.|
|\<SetID>|Especifica a ID do conjunto de cópia de sombra.|

## <a name="remarks"></a>Comentários

-   Os volumes expostos, como eles se originam, as cópias de sombra são somente leitura por padrão.
-   O alias da ID da cópia sombra, que é armazenado como uma variável de ambiente pela **carregar metadados** de comando, pode ser usado na *SetID* parâmetro.

## <a name="BKMK_examples"></a>Exemplos

Para fazer uma sombra copiar com o nome do alias Alias1 acessível como um volume gravável no sistema operacional, digite:
```
break writable %Alias1%
```

> [!NOTE]
> Acesso ao volume é feito diretamente para o provedor de hardware sem registro do volume tendo sido uma cópia de sombra.

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)