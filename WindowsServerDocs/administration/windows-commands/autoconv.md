---
title: autoconv
description: Artigo de referência para o comando autoconv, que converte FAT (tabela de alocação de arquivos) e volumes FAT32 no sistema de arquivos NTFS.
ms.topic: reference
ms.assetid: 17281e54-0b18-4e84-94ac-24586c82df4e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3d69d70200b4885404486bc4956903a17b329630
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028934"
---
# <a name="autoconv"></a>autoconv

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Converte os volumes FAT (tabela de alocação de arquivo) e FAT32 no sistema de arquivos NTFS, deixando os arquivos e diretórios existentes intactos na inicialização após a execução do **autochk** . os volumes convertidos no sistema de arquivos NTFS não podem ser convertidos de volta para FAT ou FAT32.

> [!IMPORTANT]
> Não é possível executar **autoconv** na linha de comando. Isso só pode ser executado na inicialização, se definido por meio de **convert.exe**.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando de autochk](autochk.md)

- [comando Convert](convert.md)
