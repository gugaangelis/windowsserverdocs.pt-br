---
ms.assetid: 835fc6f1-cc84-4189-b29a-dde90792469e
title: fsutil hardlink
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 69474bd1817471176598afba508cd80c8fa1df8a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840047"
---
# <a name="fsutil-hardlink"></a>fsutil hardlink
>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Cria um link físico entre um arquivo existente e um novo arquivo.

## <a name="syntax"></a>Sintaxe

```
fsutil hardlink create <NewFileName> <ExistingFileName>
fsutil hardlink list <Filename>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------------|---------------|
|criar|Estabelece um vínculo físico do NTFS entre um arquivo existente e um novo arquivo. (Um vínculo físico NTFS é semelhante a um link físico do POSIX.)|
|\<NewFileName>|Especifica o arquivo que você deseja criar um link físico para.|
|\<ExistingFileName>|Especifica o que você deseja criar um link físico do arquivo.|
|lista|Lista de links físicos para *Filename*.<br /><br />Esse parâmetro se aplica a:  Windows Server 2008 R2 e Windows 7.|

## <a name="remarks"></a>Comentários

-   Um link físico é uma entrada de diretório para um arquivo. Todos os arquivos podem ser considerados para ter pelo menos um link físico. Em volumes NTFS, cada arquivo pode ter vários links físicos, portanto, um único arquivo pode aparecer em várias pastas (ou até mesmo no mesmo diretório com nomes diferentes). Porque todos os links de referência no mesmo arquivo, programas podem abrir qualquer um dos links e modificar o arquivo. Um arquivo é excluído do sistema de arquivos somente depois que todos os links para ele tem sido excluídos. Depois de criar um link físico, programas podem usá-lo como qualquer outro nome de arquivo.

#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


