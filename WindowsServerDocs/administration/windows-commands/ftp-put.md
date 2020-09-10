---
title: ftp put
description: Artigo de referência para o comando FTP put, que copia um arquivo local para o computador remoto usando o tipo de transferência de arquivo atual.
ms.topic: reference
ms.assetid: 95cc1e3f-523d-4374-98b8-16e6c276b2ca
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 03/30/2020
ms.openlocfilehash: 24877dc154d7e88796eb3fc685351eee0c38c377
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89624731"
---
# <a name="ftp-put"></a>ftp put

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia um arquivo local para o computador remoto usando o tipo de transferência de arquivo atual.

> [!NOTE]
> Esse comando é o mesmo que o [comando FTP Send](ftp-send_1.md).

## <a name="syntax"></a>Sintaxe

```
put <localfile> [<remotefile>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<localfile>` | Especifica o arquivo local a ser copiado. |
| `[<remotefile>]` | Especifica o nome a ser usado no computador remoto. Se você não especificar um *remotefile*, o arquivo será fornecer o nome de *LocalFile* .|

### <a name="examples"></a>Exemplos

Para copiar o arquivo local *test.txt* e nomeá-lo *test1.txt* no computador remoto, digite:

```
put test.txt test1.txt
```

Para copiar o arquivo local *program.exe* para o computador remoto, digite:

```
put program.exe
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando FTP ASCII](ftp-ascii.md)

- [comando Binary FTP](ftp-binary.md)

- [Diretrizes adicionais de FTP](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
