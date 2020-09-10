---
title: bitsadmin addfile
description: Artigo de referência para o comando Bitsadmin AddFile, que adiciona um arquivo ao trabalho especificado.
ms.topic: reference
ms.assetid: 1b31aa93-0364-465b-af36-754968825989
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 74e77f2af220a057ed0ad87da797e9ae88c7aef5
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632739"
---
# <a name="bitsadmin-addfile"></a>bitsadmin addfile

Adiciona um arquivo ao trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /addfile <job> <remoteURL> <localname>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| remoteURL | A URL do arquivo no servidor. |
| localname | O nome do arquivo no computador local. *LocalName* deve conter um caminho absoluto para o arquivo. |

## <a name="examples"></a>Exemplos

Para adicionar um arquivo ao trabalho:

```
bitsadmin /addfile myDownloadJob http://downloadsrv/10mb.zip c:\10mb.zip
```

Repita essa chamada para cada arquivo a ser adicionado. Se vários trabalhos usarem *myDownloadJob* como seu nome, você deverá substituir *myDownloadJob* pelo GUID do trabalho para identificar exclusivamente o trabalho.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
