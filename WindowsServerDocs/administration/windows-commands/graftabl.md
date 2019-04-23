---
title: graftabl
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b08351d4-3d24-490c-86f6-1252da11d923
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 395873cf3dbeb574dd9abc69f45b410bece80c25
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848437"
---
# <a name="graftabl"></a>graftabl



Permite que os sistemas operacionais Windows exibir caracteres estendidos no modo gráfico. Se usado sem parâmetros, **graftabl** exibe a página de código atual e a anterior.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
graftabl <CodePage>
graftabl /status
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<CodePage>|Especifica uma página de código para definir a aparência dos caracteres estendidos em modo gráfico.</br>Números de identificação de página de código válidos são:</br>437: Estados Unidos</br>850: Multilíngue (Latino I)</br>852: Eslavo (Latim II)</br>855: Cirílico (Russo)</br>857: Turco</br>860: Português</br>861: islandês</br>863: Francês (Canadá)</br>865: Nórdico</br>866: Russo</br>869: Grego moderno|
|/status|Exibe a atual página de código **graftabl** está usando.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   **Graftabl** afeta somente a exibição de monitor de caracteres estendidos da página de código que você especificar. Ele não altera a página de código de entrada do console real. Para alterar a página de código de entrada do console, use o **modo** ou **chcp** comando.
-   A tabela a seguir lista cada código de saída e uma breve descrição dele.  
    |Código de Saída|Descrição|
    |---------|-----------|
    |0|Conjunto de caracteres foi carregado com êxito. Nenhuma página de código anterior foi carregada.|
    |1|Um parâmetro incorreto foi especificado. Nenhuma ação executada.|
    |2|Ocorreu um erro de arquivo.|
-   Você pode usar a variável de ambiente ERRORLEVEL em um arquivo em lotes para processar códigos de saída que são retornados pelo **graftabl**.

## <a name="BKMK_examples"></a>Exemplos

Para exibir a página de código atual usada pelo **graftabl**, tipo:
```
graftabl /status
```
Para carregar o conjunto de caracteres gráficos para a página de código 437 (Estados Unidos) na memória, digite:
```
graftabl 437
```
Para carregar o conjunto de página de código 850 (multilíngue) na memória de caracteres de elementos gráficos, digite:
```
graftabl 850
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)

[freedisk](freedisk.md)

[Chcp](chcp.md)