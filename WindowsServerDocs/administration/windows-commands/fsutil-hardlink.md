---
ms.assetid: 835fc6f1-cc84-4189-b29a-dde90792469e
title: Fsutil hardlink
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: de9a04850555b11a42d74826685c249bea15710c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376880"
---
# <a name="fsutil-hardlink"></a>Fsutil hardlink
>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Cria um vínculo físico entre um arquivo existente e um novo arquivo.

## <a name="syntax"></a>Sintaxe

```
fsutil hardlink create <NewFileName> <ExistingFileName>
fsutil hardlink list <Filename>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------------|---------------|
|criar|Estabelece um vínculo físico NTFS entre um arquivo existente e um novo arquivo. (Um link físico NTFS é semelhante a um vínculo físico POSIX.)|
|\<NewFileName >|Especifica o arquivo para o qual você deseja criar um link físico.|
|\<ExistingFileName >|Especifica o arquivo do qual você deseja criar um link físico.|
|lista|Lista o links físicos para *nome de arquivo*.<br /><br />Esse parâmetro se aplica a:  Windows Server 2008 R2 e Windows 7.|

## <a name="remarks"></a>Comentários

-   Um vínculo físico é uma entrada de diretório para um arquivo. Cada arquivo pode ser considerado com pelo menos um link físico. Em volumes NTFS, cada arquivo pode ter vários links físicos, de modo que um único arquivo pode aparecer em vários diretórios (ou mesmo no mesmo diretório com nomes diferentes). Como todos os links fazem referência ao mesmo arquivo, os programas podem abrir qualquer um dos links e modificar o arquivo. Um arquivo é excluído do sistema de arquivos somente depois que todos os links para ele tiverem sido excluídos. Depois de criar um link físico, os programas podem usá-lo como qualquer outro nome de arquivo.

#### <a name="additional-references"></a>Referências adicionais
[Chave da sintaxe de linha de comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


