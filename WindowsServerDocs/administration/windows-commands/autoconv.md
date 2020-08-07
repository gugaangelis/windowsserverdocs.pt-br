---
title: autoconv
description: Artigo de referência para o comando autoconv, que converte FAT (tabela de alocação de arquivos) e volumes FAT32 no sistema de arquivos NTFS.
ms.topic: article
ms.assetid: 17281e54-0b18-4e84-94ac-24586c82df4e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9f4c7ed1a2c370de46e02130fd06e1b9326207e9
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87895263"
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
