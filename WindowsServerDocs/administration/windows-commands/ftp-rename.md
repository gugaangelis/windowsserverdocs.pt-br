---
title: ftp rename
description: Artigo de referência para o comando renomear FTP, que renomeia arquivos remotos.
ms.topic: reference
ms.assetid: 977b7c95-6428-4980-80ec-79c3ae7e8c4d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a722605e451fff3ea8d4a758434a7509deaf355c
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89035734"
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
