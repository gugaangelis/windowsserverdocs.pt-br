---
title: expand
description: Artigo de referência para o comando de expansão, que expande um ou mais arquivos compactados.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 66de0488-a0c4-40c2-9b03-e40c107ba343
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 918d629ea0d3d87ea42184580c1be84740d4f79d
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86958228"
---
# <a name="expand"></a>expand

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Expande um ou mais arquivos compactados. Você também pode usar esse comando para recuperar arquivos compactados dos discos de distribuição.

O comando **expandir** também pode ser executado no console de recuperação do Windows, usando parâmetros diferentes. Para obter mais informações, consulte [ambiente de recuperação do Windows (WinRE)](/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference).

## <a name="syntax"></a>Sintaxe

```
expand [/r] <source> <destination>
expand /r <source> [<destination>]
expand /i <source> [<destination>]
expand /d <source>.cab [/f:<files>]
expand <source>.cab /f:<files> <destination>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| /r | Renomeia arquivos expandidos. |
| source | Especifica os arquivos a serem expandidos. A *origem* pode consistir em uma letra de unidade e dois-pontos, um nome de diretório, um nome de arquivo ou uma combinação desses. Você pode usar caracteres curinga (**&#42;** ou **?**). |
| destino | Especifica onde os arquivos devem ser expandidos.<p>Se a *fonte* consistir em vários arquivos e você não especificar **/r**, o *destino* deverá ser um diretório. O *destino* pode consistir em uma letra de unidade e dois-pontos, um nome de diretório, um nome de arquivo ou uma combinação desses. Especificação de destino `file | path` . |
| /i | Renomeia arquivos expandidos, mas ignora a estrutura de diretório. |
| /d | Exibe uma lista de arquivos no local de origem. Não expande nem extrai os arquivos. |
| f`<files>` | Especifica os arquivos em um arquivo de gabinete (. cab) que você deseja expandir. Você pode usar caracteres curinga (**&#42;** ou **?**). |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
