---
title: autoconv
description: Tópico de comandos do Windows para **autoconv**, que converte FAT (tabela de alocação de arquivos) e volumes FAT32 no sistema de arquivos NTFS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 17281e54-0b18-4e84-94ac-24586c82df4e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fe0e388a1d4fd79567ef0562197e3181bbbc46f4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851109"
---
# <a name="autoconv"></a>autoconv

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Converte os volumes FAT (tabela de alocação de arquivo) e FAT32 no sistema de arquivos NTFS, deixando os arquivos e diretórios existentes intactos na inicialização após a execução do **autochk** . os volumes convertidos no sistema de arquivos NTFS não podem ser convertidos de volta para FAT ou FAT32.

## <a name="remarks"></a>Comentários

Não é possível executar **autoconv** na linha de comando. Isso só será executado na inicialização, se definido por meio de **Convert. exe**.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [autochk](autochk.md)

- [convert](convert.md)

- [Trabalhando com sistemas de arquivos](https://go.microsoft.com/fwlink/?LinkId=4509)
