---
title: break
description: O tópico de comandos do Windows para break_2, que Desassocia um volume de cópia de sombra do VSS e o torna acessível como um volume regular.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: de2b6c95-1c2e-4a43-bec5-341a9014371b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6683c44c84f4baae5f016f7df62d5bd6591cff70
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848249"
---
# <a name="break"></a>break

Desassocia um volume de cópia de sombra do VSS e o torna acessível como um volume regular. O volume pode então ser acessado usando uma letra da unidade (se atribuída) ou o nome do volume. Se usado sem parâmetros, **Break** exibe a ajuda no prompt de comando.

> [!NOTE]
> Esse comando é relevante apenas para cópias de sombra de hardware após a importação.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
break [writable] <SetID>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|escrita|Habilita o acesso de leitura/gravação no volume.|
|\<SetID >|Especifica a ID do conjunto de cópias de sombra.|

## <a name="remarks"></a>Comentários

-   Os volumes expostos, como as cópias de sombra de origem, são somente leitura por padrão.
-   O alias da ID da cópia de sombra, que é armazenado como uma variável de ambiente pelo comando **carregar metadados** , pode ser usado no parâmetro *SetID* .

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para fazer uma cópia de sombra com o nome do alias Alias1 acessível como um volume gravável no sistema operacional, digite:
```
break writable %Alias1%
```

> [!NOTE]
> O acesso ao volume é feito diretamente para o provedor de hardware sem registro do volume que tem sido uma cópia de sombra.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)