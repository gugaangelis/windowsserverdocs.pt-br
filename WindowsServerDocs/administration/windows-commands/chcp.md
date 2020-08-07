---
title: chcp
description: Artigo de referência para o comando CHCP, que altera a página de código ativa do console.
ms.topic: article
ms.assetid: dc7b1c71-7b80-443d-9cf1-9bcf305aa1fd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 21dcdc1e663656439bece576287877653d0dcd8c
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87892811"
---
# <a name="chcp"></a>chcp

Altera a página de código do console ativo. Se usado sem parâmetros, **chcp** exibe o número da página de código do console ativo.

## <a name="syntax"></a>Sintaxe

```
chcp [<nnn>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<nnn>` | Especifica a página de código. |
| /? | Exibe a ajuda no prompt de comando. |

A tabela a seguir lista cada página de código com suporte e seu país/região ou idioma:

| Página de código | País/região ou idioma |
| --------- | -------------------------- |
| 437 | Estados Unidos |
| 850 | Multilíngue (Latino I) |
| 852 | Eslavo (latino II) |
| 855 | Cirílico (russo) |
| 857 | Turco |
| 860 | Português |
| 861 | Islandês |
| 863 | Canadá-francês |
| 865 | Nórdico |
| 866 | Russo |
| 869 | Grego moderno |
| 936 | Chinês |

#### <a name="remarks"></a>Comentários

- Somente a página de código OEM (fabricante original do equipamento) instalada com o Windows aparece corretamente em uma janela de prompt de comando que usa fontes de varredura. Outras páginas de código aparecem corretamente no modo de tela inteira ou em janelas de prompt de comando que usam fontes TrueType.

- Você não precisa preparar as páginas de código (como no MS-DOS).

- Os programas que você inicia depois de atribuir uma nova página de código usam a nova página de código. No entanto, os programas (exceto Cmd.exe) que você iniciou antes de atribuir a nova página de código continuarão a usar a página de código original.

## <a name="examples"></a>Exemplos

Para exibir a configuração da página de código ativa, digite:

```
chcp
```

É exibida uma mensagem semelhante à seguinte:`Active code page: 437`

Para alterar a página de código ativa para 850 (multilíngue), digite:

```
chcp 850
```

Se a página de código especificada for inválida, a seguinte mensagem de erro será exibida:`Invalid code page`

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
