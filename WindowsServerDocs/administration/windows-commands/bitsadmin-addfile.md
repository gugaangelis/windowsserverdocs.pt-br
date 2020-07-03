---
title: bitsadmin addfile
description: Artigo de referência para o comando Bitsadmin AddFile, que adiciona um arquivo ao trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1b31aa93-0364-465b-af36-754968825989
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 817a6d3af81d88e571db17c3f57dc129130b2783
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927103"
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
