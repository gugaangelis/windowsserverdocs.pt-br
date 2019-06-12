---
title: autochk
description: Tópico de comandos do Windows para **autochk** - é executado quando o computador é iniciado e antes do Windows Server iniciando verificar a integridade lógica de um sistema de arquivos.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8787e6a3-f023-4ea5-b2d1-61c6876d8aff
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e6c26d42410e5466950ede4f9aa059e315030588
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435031"
---
# <a name="autochk"></a>autochk



Executa quando o computador é iniciado e antes do Windows Server® 2008 R2 iniciando verificar a integridade lógica de um sistema de arquivos.

**Autochk.exe** é uma versão do **Chkdsk** que é executado somente em discos NTFS e apenas antes do início do Windows Server 2008 R2. **Autochk** não podem ser executados diretamente na linha de comando. Em vez disso, **Autochk** é executado nas seguintes situações:
-   Se você tentar executar **Chkdsk** no volume de inicialização
-   Se **Chkdsk** não é possível obter uso exclusivo do volume
-   Se o volume é marcado como sujo

## <a name="remarks"></a>Comentários

> -   [!WARNING]
>     O **Autochk** ferramenta de linha de comando não pode ser executada diretamente na linha de comando. Em vez disso, use o **Chkntfs** ferramenta de linha de comando para configurar o modo como você deseja **Autochk** para executar na inicialização.
> -   Você pode usar **Chkntfs** com o **/x** parâmetro para evitar **Autochk** seja executado em um volume específico ou vários volumes.
> -   Use o **Chkntfs.exe** ferramenta de linha de comando com o **/t** parâmetro para alterar o atraso Autochk de 0 segundos para até 3 dias (259.200 segundos). No entanto, um longo atraso significa que o computador não é iniciado até que tenha decorrido o tempo ou até que você pressione uma tecla para cancelar **Autochk**.

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)

[Chkdsk](chkdsk.md)

[Chkntfs](chkntfs.md)

[Discos e sistemas de arquivos de solução de problemas](https://go.microsoft.com/fwlink/?LinkId=4527)