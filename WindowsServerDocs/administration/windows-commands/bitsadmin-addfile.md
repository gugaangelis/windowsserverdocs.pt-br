---
title: bitsadmin addfile
description: O tópico de comandos do Windows para **Bitsadmin AddFile**, que adiciona um arquivo ao trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1b31aa93-0364-465b-af36-754968825989
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 330e79eb2ba5a824cea54094f64ceb6f9cfd66b9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850959"
---
# <a name="bitsadmin-addfile"></a>bitsadmin addfile

Adiciona um arquivo ao trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /AddFile <Job> <RemoteURL> <LocalName>
```

#### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| Trabalho | O nome de exibição ou o GUID do trabalho. |
| RemoteURL | A URL do arquivo no servidor. |
| LocalName | O nome do arquivo no computador local. *LocalName* deve conter um caminho absoluto para o arquivo. |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Adicione um arquivo ao trabalho. Repita essa chamada para cada arquivo que você deseja adicionar. Se vários trabalhos usarem *myDownloadJob* como seu nome, você deverá substituir *myDownloadJob* pelo GUID do trabalho para identificar exclusivamente o trabalho.

```
C:\>bitsadmin /addfile myDownloadJob http://downloadsrv/10mb.zip c:\10mb.zip
```

## <a name="additional-references"></a>Referências adicionais

- [Chave de sintaxe de linha de comando](command-line-syntax-key.md)&copy;