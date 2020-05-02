---
title: bitsadmin getsecurityflags
description: Tópico de referência para o comando Bitsadmin getsecurityflags, que relata os sinalizadores de segurança HTTP para o redirecionamento de URL e as verificações executadas no certificado do servidor durante a transferência.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c2e73519-34f4-487b-af11-97d5d08ef9bb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 41b710f9897f24eb4161d9379dc3b1f89b141472
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717547"
---
# <a name="bitsadmin-getsecurityflags"></a>bitsadmin getsecurityflags

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Relata os sinalizadores de segurança HTTP para o redirecionamento de URL e as verificações executadas no certificado do servidor durante a transferência.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getsecurityflags <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar os sinalizadores de segurança de um trabalho chamado *myDownloadJob*:

```
bitsadmin /getsecurityflags myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
