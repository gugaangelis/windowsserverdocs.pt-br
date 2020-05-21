---
title: fsutil hardlink
description: Tópico de referência para o comando fsutil hardlink, que cria um vínculo físico entre um arquivo existente e um novo arquivo.
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: 835fc6f1-cc84-4189-b29a-dde90792469e
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: ef0f8347a73a2522f6c4b9298799ad2e3536c4c9
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2020
ms.locfileid: "83435921"
---
# <a name="fsutil-hardlink"></a>fsutil hardlink

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Cria um vínculo físico entre um arquivo existente e um novo arquivo. Um vínculo físico é uma entrada de diretório para um arquivo. Cada arquivo pode ser considerado com pelo menos um link físico.

Em volumes NTFS, cada arquivo pode ter vários links físicos, de modo que um único arquivo pode aparecer em vários diretórios (ou mesmo no mesmo diretório com nomes diferentes). Como todos os links fazem referência ao mesmo arquivo, os programas podem abrir qualquer um dos links e modificar o arquivo. Um arquivo é excluído do sistema de arquivos somente depois que todos os links para ele tiverem sido excluídos. Depois de criar um link físico, os programas podem usá-lo como qualquer outro nome de arquivo.

## <a name="syntax"></a>Sintaxe

```
fsutil hardlink create <newfilename> <existingfilename>
fsutil hardlink list <filename>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| create | Estabelece um vínculo físico NTFS entre um arquivo existente e um novo arquivo. (Um link físico NTFS é semelhante a um vínculo físico POSIX.) |
| \<NewFileName> | Especifica o arquivo para o qual você deseja criar um link físico. |
| \<> existingfilename | Especifica o arquivo do qual você deseja criar um link físico. |
| list | Lista os links físicos para *nome de arquivo*. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [fsutil](fsutil.md)
