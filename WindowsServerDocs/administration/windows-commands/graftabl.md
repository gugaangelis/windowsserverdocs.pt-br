---
title: graftabl
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b08351d4-3d24-490c-86f6-1252da11d923
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b3815b85da163c03dea7bd2619d4647454d3ebd7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724921"
---
# <a name="graftabl"></a>graftabl



Permite que os sistemas operacionais Windows exibam um conjunto de caracteres estendidos no modo gráfico. Se usado sem parâmetros, **GRAFTABL** exibirá a página de código anterior e a atual.



## <a name="syntax"></a>Sintaxe

```
graftabl <CodePage>
graftabl /status
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<> de página de código|Especifica uma página de código para definir a aparência de caracteres estendidos no modo gráfico.</br>Os números de identificação de página de código válidos são:</br>437: Estados Unidos</br>850: multilíngue (I latino)</br>852: eslavo (latino II)</br>855: cirílico (russo)</br>857: Turco</br>860: Português</br>861: Islandês</br>863: Canadá – francês</br>865: nórdico</br>866: Russo</br>869: grego moderno|
|/status|Exibe a página de código atual que **GRAFTABL** está usando.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   **GRAFTABL** afeta apenas a exibição do monitor de caracteres estendidos da página de código que você especificar. Ele não altera a página de código de entrada do console real. Para alterar a página de código de entrada do console, use o comando **Mode** ou **chcp** .
-   A tabela a seguir lista cada código de saída e uma breve descrição dele.  
    |Código de saída|Descrição|
    |---------|-----------|
    |0|O conjunto de caracteres foi carregado com êxito. Nenhuma página de código anterior foi carregada.|
    |1|Um parâmetro incorreto foi especificado. Nenhuma ação executada.|
    |2|Ocorreu um erro de arquivo.|
-   Você pode usar a variável de ambiente ERRORLEVEL em um programa em lotes para processar códigos de saída retornados por **GRAFTABL**.

## <a name="examples"></a>Exemplos

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

[Freedisk](freedisk.md)

[Chcp](chcp.md)