---
title: autoconv
description: O tópico de comandos do Windows para **autoconv** -converte a tabela de alocação de arquivo (FAT) e os volumes FAT32 no sistema de arquivos NTFS.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 17281e54-0b18-4e84-94ac-24586c82df4e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bf36be6bcf3dd8f6c61c6ab0d8780ed77dd8903a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383452"
---
# <a name="autoconv"></a>autoconv

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Converte os volumes FAT (tabela de alocação de arquivo) e FAT32 no sistema de arquivos NTFS, deixando os arquivos e diretórios existentes intactos na inicialização após a execução do **autochk** . os volumes convertidos no sistema de arquivos NTFS não podem ser convertidos de volta para FAT ou FAT32.
## <a name="remarks"></a>Comentários
Não é possível executar **autoconv** na linha de comando. Isso só será executado na inicialização, se definido por meio de **Convert. exe**.
## <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[autochk](autochk.md)
[converta](convert.md)
[trabalhando com sistemas de arquivos](https://go.microsoft.com/fwlink/?LinkId=4509)
