---
ms.assetid: fb95c8ee-a418-4520-a12a-7754ae947c3c
title: fsutil reparsepoint
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: f66f09fa608fec10d7126e516f9cf2dd8a19bbfb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438990"
---
# <a name="fsutil-reparsepoint"></a>fsutil reparsepoint
>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7, Windows 2008, Windows Vista

Consultas ou exclusões de nova análise pontos.  O **fsutil reparsepoint** comando normalmente é usado por profissionais de suporte.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
fsutil reparsepoint [query] <FileName>
fsutil reparsepoint [delete] <FileName>
```

## <a name="parameters"></a>Parâmetros

| Parâmetro  |                                                                Descrição                                                                |
|------------|-------------------------------------------------------------------------------------------------------------------------------------------|
|   consulta    |            Recupera os dados de ponto de nova análise que está associados com o arquivo ou diretório identificada pelo identificador especificado.             |
|   excluir   | Exclui um ponto de nova análise do arquivo ou diretório que é identificado pelo identificador especificado, mas não exclui o arquivo ou diretório. |
| <FileName> |             Especifica o caminho completo para o arquivo, incluindo o nome de arquivo e extensão, por exemplo C:\documents\filename.txt.             |

## <a name="remarks"></a>Comentários

-   Pontos de nova análise é objetos de sistema que têm um atributo definível que contém os dados definidos pelo usuário de arquivo NTFS, e eles são usados para estender a funcionalidade no subsistema de e/s () de entrada/saída.

-   Nova análise pontos são usados para pontos de junção de diretório e os pontos de montagem de volume. Eles também são usados por drivers de filtro do sistema de arquivos para marcar determinados arquivos como especiais para esse driver.

-   Quando um programa define um ponto de nova análise, ele armazena esses dados, além de uma marca de nova análise, que identifica exclusivamente os dados que estão sendo armazenados. Quando o sistema de arquivos abre um arquivo com um ponto de nova análise, ele tenta localizar o filtro de sistema de arquivo associado. Se o filtro de sistema de arquivos for encontrado, o filtro processa o arquivo conforme indicado pelos dados de nova análise. Se nenhum filtro de sistema de arquivos for encontrado, a operação de abertura do arquivo falhará.

## <a name="BKMK_examples"></a>Exemplos
Para recuperar dados de ponto de nova análise associados a C:\Server, digite:

```
fsutil reparsepoint query c:\server
```

Para excluir um ponto de nova análise de um arquivo ou diretório especificado, use o seguinte formato:

```
fsutil reparsepoint delete c:\server
```

#### <a name="additional-references"></a>Referências adicionais
[Chave da sintaxe de linha de comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


