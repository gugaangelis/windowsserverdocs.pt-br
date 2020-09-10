---
title: bitsadmin getclientcertificate
description: Artigo de referência para o comando Bitsadmin GetClientCertificate, que recupera o certificado do cliente do trabalho.
ms.topic: reference
ms.assetid: 4fc8f408-085e-43a0-9fa8-3d798ef107b1
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 71f1ed8920ba2bd3aa0a0f48683d8841e08afb6d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632263"
---
# <a name="bitsadmin-getclientcertificate"></a>bitsadmin getclientcertificate

Recupera o certificado do cliente do trabalho.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getclientcertificate <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar o certificado do cliente para o trabalho chamado *myDownloadJob*:

```
bitsadmin /getclientcertificate myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
