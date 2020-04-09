---
title: mmc
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7bfa4030-ce42-40fb-922f-2f5145a80872
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0d143336db3369b3b319391967db879126d2fb29
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839439"
---
# <a name="mmc"></a>mmc

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Usando opções de linha de comando do MMC, você pode abrir um console do **MMC** específico, abrir o **MMC** no modo de autor ou especificar que a versão de 32 bits ou 64 bits do **MMC** esteja aberta.
## <a name="syntax"></a>Sintaxe
```
mmc <path>\<filename>.msc [/a] [/64] [/32]
```
#### <a name="parameters"></a>Parâmetros

|       Parâmetro        |                                                                                                 Descrição                                                                                                 |
|------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <path>\\<filename>. msc |        inicia o **MMC** e abre um console salvo. Você precisa especificar o caminho completo e o nome do arquivo do console salvo. Se você não especificar um arquivo de console, o **MMC** abrirá um novo console.         |
|           /a           |                                                               Abre um console salvo no modo de autor.  Usado para fazer alterações em consoles salvos.                                                                |
|          /64           |                         Abre a versão de 64 bits do **MMC** (MMC64). Use esta opção somente se você estiver executando um sistema operacional Microsoft de 64 bits e quiser usar um snap-in de 64 bits.                          |
|          /32           | Abre a versão de 32 bits do **MMC** (MMC32). Ao executar um sistema operacional Microsoft de 64 bits, você pode executar snap-ins de 32 bits abrindo o MMC com essa opção de linha de comando quando tiver snap-ins somente de 32 bits. |
|           /?           |                                                                                    Exibe a ajuda no prompt de comando.                                                                                     |

## <a name="remarks"></a>Comentários
- Usando a opção de linha de comando <path> **\\** <filename> **. msc** , você pode usar variáveis de ambiente para criar linhas de comando ou atalhos que não dependem do local explícito dos arquivos de console. Por exemplo, se o caminho para um arquivo de console estiver na pasta do sistema (por exemplo, **MMC c:\winnt\system32\ console_name. msc**), você poderá usar a cadeia de caracteres de dados expansível **% systemroot%** para especificar o local (**mmc% systemroot% \ system32 \ console_name. msc**). Isso pode ser útil se você estiver delegando tarefas a pessoas em sua organização que estão trabalhando em computadores diferentes.
- Usando a opção de linha de comando **/A** quando os consoles são abertos com essa opção, eles são abertos no modo de autor, independentemente do seu modo padrão. Isso não altera permanentemente a configuração de modo padrão para arquivos; Quando você omite essa opção, o MMC abre arquivos de console de acordo com suas configurações de modo padrão.
- Depois de abrir o **MMC** ou um arquivo de console no modo de autor, você pode abrir qualquer console existente clicando em **abrir** no menu do **console** .
- Você pode usar a linha de comando para criar atalhos para abrir o **MMC** e os consoles salvos. Um comando de linha de comando funciona com o comando **executar** no menu **Iniciar** , em qualquer janela de prompt de comando, em atalhos ou em qualquer arquivo ou programa em lotes que chame o comando.
  ## <a name="additional-references"></a>Referências adicionais
- - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

