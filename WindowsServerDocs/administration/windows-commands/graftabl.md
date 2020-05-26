---
title: graftabl
description: Tópico de referência para o comando GRAFTABL, que permite que os sistemas operacionais Windows exibam um conjunto de caracteres estendidos no modo gráfico.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b08351d4-3d24-490c-86f6-1252da11d923
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 149ae92db534cef66c966462e51906304588b042
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2020
ms.locfileid: "83818686"
---
# <a name="graftabl"></a>graftabl

Permite que os sistemas operacionais Windows exibam um conjunto de caracteres estendidos no modo gráfico. Se usado sem parâmetros, **GRAFTABL** exibirá a página de código anterior e a atual.

## <a name="syntax"></a>Sintaxe

```
graftabl <codepage>
graftabl /status
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<codepage>` | Especifica uma página de código para definir a aparência de caracteres estendidos no modo gráfico. Os números de identificação de página de código válidos são:<ul><li>**437** -Estados Unidos</li><li>**850** – multilíngue (Latino I)</li><li>**852** -eslavo (latino II)</li><li>**855** -cirílico (russo)</li><li>**857** -Turco</li><li>**860** -Português</li><li>**861** – Islandês</li><li>**863** -Canadá-francês</li><li>**865** -nórdico</li><li>**866** – russo</li><li>**869** -grego moderno</li></ul> |
| /status | Exibe a página de código atual que está sendo usada por este comando. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- O comando **GRAFTABL** afeta apenas a exibição do monitor de caracteres estendidos da página de código que você especificar. Ele não altera a página de código de entrada do console real. Para alterar a página de código de entrada do console, use o comando [Mode](mode.md) ou [chcp](chcp.md) .

- Cada código de saída e uma breve descrição dele:

    | Código de saída | Descrição |
    | --------- | ----------- |
    | 0 | O conjunto de caracteres foi carregado com êxito. Nenhuma página de código anterior foi carregada. |
    | 1 | Um parâmetro incorreto foi especificado. Nenhuma ação executada. |
    | 2 | Ocorreu um erro de arquivo. |

- Você pode usar a variável de ambiente ERRORLEVEL em um programa em lotes para processar códigos de saída retornados por **GRAFTABL**.

### <a name="examples"></a>Exemplos

Para exibir a página de código atual usada por **GRAFTABL**, digite:

```
graftabl /status
```

Para carregar o conjunto de caracteres gráficos para a página de código 437 (Estados Unidos) na memória, digite:

```
graftabl 437
```

Para carregar o conjunto de caracteres gráficos para a página de código 850 (multilíngue) na memória, digite:

```
graftabl 850
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando freedisk](freedisk.md)

- [comando de modo](mode.md)

- [comando CHCP](chcp.md)
