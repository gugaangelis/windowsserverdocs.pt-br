---
ms.assetid: 77545920-2d13-4f35-a4d1-14dbec8340dc
title: Fsutil SPARSE
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 70c881cc02f31614160920766d32d73a3a939315
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844059"
---
# <a name="fsutil-sparse"></a>Fsutil SPARSE
>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Gerencia arquivos esparsos.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
fsutil sparse [queryflag] <FileName>
fsutil sparse [queryrange] <FileName>
fsutil sparse [setflag] <FileName>
fsutil sparse [setrange] <FileName> <BeginningOffset> <Length>
```

### <a name="parameters"></a>Parâmetros

|     Parâmetro     |                                                    Descrição                                                    |
|-------------------|-------------------------------------------------------------------------------------------------------------------|
|     queryflag     |                                                  Consultas esparsas.                                                  |
|    queryrange     |                        Examina um arquivo e procura intervalos que possam conter dados diferentes de zero.                        |
|      SetFlag      |                                        Marca o arquivo indicado como esparso.                                        |
|     SetRange      |                                   Preenche um intervalo especificado de um arquivo com zeros.                                   |
|    <FileName>     | Especifica o caminho completo para o arquivo, incluindo o nome do arquivo e a extensão, por exemplo, C:\documents\filename.txt. |
| <BeginningOffset> |                              Especifica o deslocamento dentro do arquivo a ser marcado como esparso.                              |
|     <Length>      |                 Especifica o comprimento da região no arquivo a ser marcado como esparso (em bytes).                 |

## <a name="remarks"></a>Comentários

-   Um arquivo esparso é um arquivo com uma ou mais regiões de dados não alocados nele. Um programa verá essas regiões não alocadas como contendo bytes com o valor zero, mas nenhum espaço em disco será usado para representar esses zeros. Todos os dados significativos ou diferentes de zero são alocados, enquanto todos os dados não-significado (grandes cadeias de dados compostos por zeros) não são alocados. Quando um arquivo esparso é lido, os dados alocados são retornados como armazenados e os dados não alocados são retornados, por padrão, como zeros, de acordo com a especificação de requisito de segurança C2. O suporte a arquivos esparsos permite que os dados sejam desalocados de qualquer lugar no arquivo.

-   Em um arquivo esparso, grandes intervalos de zeros podem não exigir alocação de disco. O espaço para dados diferentes de zero será alocado conforme necessário quando o arquivo for gravado.

-   Somente arquivos compactados ou esparsos podem ter intervalos zerados conhecidos pelo sistema operacional.

-   Se o arquivo for esparso ou compactado, o NTFS poderá desalocar espaço em disco no arquivo. Isso define o intervalo de bytes como zeros sem estender o tamanho do arquivo.

## <a name="examples"></a><a name="BKMK_examples"></a>Disso
Para marcar um arquivo chamado Sample. txt no diretório C:\Temp como esparso, digite:

```
fsutil sparse setflag c:\temp\sample.txt 
```

## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

[Fsutil](Fsutil.md)


