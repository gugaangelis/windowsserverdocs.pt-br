---
ms.assetid: 77545920-2d13-4f35-a4d1-14dbec8340dc
title: fsutil esparso
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 76a263c82ebc42de4cc6d136f9a814c3a678666b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878387"
---
# <a name="fsutil-sparse"></a>fsutil esparso
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

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------------|---------------|
|queryflag|Pesquisa arquivos esparsos.|
|queryrange|Analisa um arquivo e procura por intervalos que podem conter dados diferentes de zero.|
|setflag|Marca o arquivo indicado como esparso.|
|SetRange|Preenche um intervalo especificado de um arquivo com zeros.|
|<FileName>|Especifica o caminho completo para o arquivo, incluindo o nome de arquivo e extensão, por exemplo C:\documents\filename.txt.|
|<BeginningOffset>|Especifica o deslocamento dentro do arquivo para marcar como esparso.|
|<Length>|Especifica o comprimento da região em que o arquivo a ser marcado como esparso (em bytes).|

## <a name="remarks"></a>Comentários

-   Um arquivo esparso é um arquivo com uma ou mais regiões de dados não alocados. Um programa verá essas regiões não alocados como contendo bytes com o valor de zero, mas nenhum espaço em disco é usado para representar esses zeros. Todos os dados significativos ou diferente de zero é alocado, ao passo que todos os dados nonmeaningful (cadeias de caracteres grandes de dados que são compostos de zeros) não está alocada. Quando um arquivo esparso é lido, dados alocados são retornados como armazenados, e não alocado de dados são retornados, por padrão, como zeros, de acordo com a especificação de requisito de segurança de C2. Suporte a arquivos esparsos permite que os dados a serem desalocados em qualquer lugar no arquivo.

-   Em um arquivo esparso, grandes intervalos de zeros podem não exigir a alocação de disco. Espaço de dados diferente de zero será alocado conforme necessário, quando o arquivo é gravado.

-   Apenas arquivos compactados ou esparsos podem ter intervalos zerados conhecidos pelo sistema operacional.

-   Se o arquivo está compactado ou esparso, NTFS pode desalocar espaço em disco no arquivo. Isso define o intervalo de bytes sem estender o tamanho do arquivo.

## <a name="BKMK_examples"></a>Exemplos
Para marcar um arquivo chamado txt no diretório C:\Temp como esparso, digite:

```
fsutil sparse setflag c:\temp\sample.txt 
```

#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


