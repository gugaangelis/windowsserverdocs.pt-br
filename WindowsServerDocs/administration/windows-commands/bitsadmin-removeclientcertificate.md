---
title: bitsadmin removeclientcertificate
description: Artigo de referência para o comando Bitsadmin removeclientcertificate, que remove o certificado do cliente do trabalho.
ms.topic: reference
ms.assetid: b417c3e5-aadd-4fcc-968f-45d8b67ca516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a447354a309d16ed75c89c586c8edabd2eadb528
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89026424"
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
