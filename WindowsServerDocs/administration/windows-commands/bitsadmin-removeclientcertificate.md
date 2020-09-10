---
title: bitsadmin removeclientcertificate
description: Artigo de referência para o comando Bitsadmin removeclientcertificate, que remove o certificado do cliente do trabalho.
ms.topic: reference
ms.assetid: b417c3e5-aadd-4fcc-968f-45d8b67ca516
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: f2c739c1e1b1b3ee31d592adfb17e363dde865bf
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631162"
---
# <a name="bitsadmin-removeclientcertificate"></a>bitsadmin removeclientcertificate

Remove o certificado do cliente do trabalho.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /removeclientcertificate <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para remover o certificado do cliente do trabalho chamado *myDownloadJob*:

```
bitsadmin /removeclientcertificate myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
