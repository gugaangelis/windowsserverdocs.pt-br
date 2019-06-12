---
title: mmc
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7bfa4030-ce42-40fb-922f-2f5145a80872
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4bdc093bd16ea08153b7dbc4a0e3380251f2ed7d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437336"
---
# <a name="mmc"></a>mmc

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Usando as opções de linha de comando do mmc, você pode abrir um determinado **mmc** console, abra **mmc** no modo de autor ou especificar que a versão de 32 bits ou 64 bits do **mmc** é aberto.
## <a name="syntax"></a>Sintaxe
```
mmc <path>\<filename>.msc [/a] [/64] [/32]
```
### <a name="parameters"></a>Parâmetros

|       Parâmetro        |                                                                                                 Descrição                                                                                                 |
|------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <path>\\<filename>.msc |        inicia **mmc** e abre um console salvo. Você precisa especificar o nome de arquivo e caminho completo para o arquivo de console salvo. Se você não especificar um arquivo de console **mmc** abre um novo console.         |
|           /a           |                                                               Abre um console salvo no modo de autor.  Usado para fazer alterações em consoles salvos.                                                                |
|          /64           |                         Abre a versão de 64 bits do **mmc** (mmc64). Use esta opção somente se você estiver executando um sistema operacional Microsoft 64-bits e deseja usar um snap-in de 64 bits.                          |
|          /32           | Abre a versão de 32 bits do **mmc** (mmc32). Ao executar um sistema operacional Microsoft 64-bits, você pode executar o snap-ins de 32 bits por abrir o mmc com essa opção de linha de comando quando você tiver 32 bits somente os snap-ins. |
|           /?           |                                                                                    Exibe a ajuda no prompt de comando.                                                                                     |

## <a name="remarks"></a>Comentários
- Usando o <path> **\\** <filename> **. msc** opção de linha de comando, você pode usar variáveis de ambiente para criar linhas de comando ou atalhos que não dependem explícita local dos arquivos de console. Por exemplo, se o caminho para um arquivo de console é na pasta do sistema (por exemplo, **mmc c:\winnt\system32\console_name.msc**), você pode usar a cadeia de caracteres de dados expansível **% systemroot %** para especificar o local (**mmc%systemroot%\system32\console_name.msc**). Isso pode ser útil se você estiver Delegando tarefas às pessoas na sua organização que trabalham em computadores diferentes.
- Usando o **/a** opção de linha de comando quando consoles são abertos com essa opção, eles são abertos no modo de autor, independentemente do seu modo padrão. Isso não altera a configuração de modo padrão de arquivos; permanentemente Quando você omitir esta opção, o mmc abre arquivos de acordo com suas configurações de modo padrão do console.
- Depois de abrir **mmc** ou um arquivo de console no modo de autor, você pode abrir qualquer console existente clicando **abra** sobre o **Console** menu.
- Você pode usar a linha de comando para criar atalhos para abertura **mmc** e consoles salvos. Um comando de linha de comando funciona com o **executados** comando o **iniciar** menu, em qualquer janela de prompt de comando, em atalhos ou em qualquer arquivo em lotes ou um programa que chama o comando.
  ## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

