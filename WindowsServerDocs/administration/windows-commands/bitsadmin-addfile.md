---
title: bitsadmin addfile
description: Tópico de referência para o comando Bitsadmin AddFile, que adiciona um arquivo ao trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1b31aa93-0364-465b-af36-754968825989
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eaa7d77c9d6160bbd2bdf6a1431232af22bc3e37
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718494"
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
