---
title: autoconv
description: Tópico de comandos do Windows para **autoconv** - converte a tabela de alocação (Fat) de arquivo e sistema de arquivos Fat32 volumes NTFS.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 1d135da085558f12a51c8febfd72aa805e1d12f1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872487"
---
# <a name="autoconv"></a>autoconv

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Converte arquivos (Fat) de tabela de alocação e volumes Fat32 ao sistema de arquivos NTFS, deixando intactos na inicialização após a arquivos e diretórios existentes **autochk** é executado. volumes convertidos para o sistema de arquivos NTFS não podem ser convertidos para Fat ou Fat32.
## <a name="remarks"></a>Comentários
Não é possível executar **autoconv** na linha de comando. Isso só será executado na inicialização, se definida por meio **convert.exe**.
## <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[autochk](autochk.md)
[converter](convert.md)
[trabalhar com sistemas de arquivos](https://go.microsoft.com/fwlink/?LinkId=4509)
