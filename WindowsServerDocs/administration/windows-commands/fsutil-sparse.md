---
title: fsutil sparse
description: Tópico de referência para o comando fsutil SPARSE, que gerencia arquivos esparsos.
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: 77545920-2d13-4f35-a4d1-14dbec8340dc
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: e68ac844bb7aa7e22a9df0ddb0c982b3701231d7
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2020
ms.locfileid: "83435711"
---
# <a name="fsutil-sparse"></a>fsutil sparse

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Gerencia arquivos esparsos. Um arquivo esparso é um arquivo com uma ou mais regiões de dados não alocados nele.

Um programa vê essas regiões não alocadas como contendo bytes com um valor zero e que não há espaço em disco representando esses zeros. Quando um arquivo esparso é lido, os dados alocados são retornados como armazenados e os dados não alocados são retornados, por padrão, como zeros, de acordo com a especificação de requisito de segurança C2. O suporte a arquivos esparsos permite que os dados sejam desalocados de qualquer lugar no arquivo.

## <a name="syntax"></a>Sintaxe

```
fsutil sparse [queryflag] <filename>
fsutil sparse [queryrange] <filename>
fsutil sparse [setflag] <filename>
fsutil sparse [setrange] <filename> <beginningoffset> <length>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| queryflag | Consultas esparsas. |
| queryrange | Examina um arquivo e procura intervalos que possam conter dados diferentes de zero. |
| SetFlag | Marca o arquivo indicado como esparso. |
| SetRange | Preenche um intervalo especificado de um arquivo com zeros. |
| `<filename>` | Especifica o caminho completo para o arquivo, incluindo o nome do arquivo e a extensão, por exemplo, *C:\documents\filename.txt*. |
| `<beginningoffset>` | Especifica o deslocamento dentro do arquivo a ser marcado como esparso. |
| `<length>` | Especifica o comprimento da região no arquivo a ser marcado como esparso (em bytes). |

#### <a name="remarks"></a>Comentários

- Todos os dados significativos ou diferentes de zero são alocados, enquanto todos os dados não significativos (cadeias de caracteres grandes de dados compostos por zeros) não são alocados.

- Em um arquivo esparso, grandes intervalos de zeros podem não exigir alocação de disco. O espaço para dados diferentes de zero é alocado conforme necessário quando o arquivo é gravado.

- Somente arquivos compactados ou esparsos podem ter intervalos zerados conhecidos pelo sistema operacional.

- Se o arquivo for esparso ou compactado, o NTFS poderá desalocar espaço em disco dentro do arquivo. Isso define o intervalo de bytes como zeros sem estender o tamanho do arquivo.

### <a name="examples"></a>Exemplos

Para marcar um arquivo chamado *Sample. txt* no diretório *c:\temp* como esparso, digite:

```
fsutil sparse setflag c:\temp\sample.txt
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [fsutil](fsutil.md)
