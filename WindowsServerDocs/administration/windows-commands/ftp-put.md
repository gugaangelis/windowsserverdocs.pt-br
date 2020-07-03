---
title: ftp put
description: Artigo de referência para o comando FTP put, que copia um arquivo local para o computador remoto usando o tipo de transferência de arquivo atual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 95cc1e3f-523d-4374-98b8-16e6c276b2ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/30/2020
ms.openlocfilehash: 7b382673be838af739c29ffa294e3d853f0507c2
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85925158"
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

- [Diretrizes adicionais de FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
