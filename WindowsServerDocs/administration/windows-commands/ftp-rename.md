---
title: ftp rename
description: Artigo de referência para o comando renomear FTP, que renomeia arquivos remotos.
ms.topic: article
ms.assetid: 977b7c95-6428-4980-80ec-79c3ae7e8c4d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c85903987be9df566f4c07bc7fb5b96e76b0aa43
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87888984"
---
# <a name="ftp-rename"></a>ftp rename

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Renomeia arquivos remotos.

## <a name="syntax"></a>Sintaxe

```
rename <filename> <newfilename>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<filename>` | Especifica o arquivo que você deseja renomear. |
| `<newfilename>` | Especifica o novo nome de arquivo. |

### <a name="examples"></a>Exemplos

Para renomear o *example.txt* de arquivo remoto para *example1.txt*, digite:

```
rename example.txt example1.txt
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Diretrizes adicionais de FTP](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
