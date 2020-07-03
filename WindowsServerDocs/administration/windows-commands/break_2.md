---
title: break
description: Artigo de referência para o comando break, que Desassocia um volume de cópia de sombra do VSS e o torna acessível como um volume regular.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: de2b6c95-1c2e-4a43-bec5-341a9014371b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7f3974f183215a42920f7406a62ab335eb101f56
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85924898"
---
# <a name="break"></a>break

Desassocia um volume de cópia de sombra do VSS e o torna acessível como um volume regular. O volume pode então ser acessado usando uma letra da unidade (se atribuída) ou o nome do volume. Se usado sem parâmetros, **Break** exibe a ajuda no prompt de comando.

> [!NOTE]
> Esse comando é relevante apenas para cópias de sombra de hardware após a importação.
>
> Os volumes expostos, como as cópias de sombra de origem, são somente leitura por padrão. O acesso ao volume é feito diretamente para o provedor de hardware sem registro do volume que tem sido uma cópia de sombra.

## <a name="syntax"></a>Sintaxe

```
break [writable] <setid>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| escrita | Habilita o acesso de leitura/gravação no volume. |
| \<setid> | Especifica a ID do conjunto de cópias de sombra. O alias da ID da cópia de sombra, que é armazenado como uma variável de ambiente pelo comando **carregar metadados** , pode ser usado no parâmetro *SetID* . |

## <a name="examples"></a>Exemplos

Para fazer uma cópia de sombra usando o nome do alias Alias1 acessível como um volume gravável no sistema operacional:

```
break writable %Alias1%
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)