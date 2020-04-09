---
title: bitsadmin takeownership
description: O tópico de comandos do Windows para Bitsadmin TakeOwnership, que permite que um usuário com privilégios administrativos assuma a propriedade do trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ea0ce7cb-440a-498f-a3ef-8368fa43e399
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4a2c0bfc1fcb1606102aece76129c49aad701ead
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849019"
---
# <a name="bitsadmin-takeownership"></a>bitsadmin takeownership

Permite que um usuário com privilégios administrativos assuma a propriedade do trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /TakeOwnership <Job>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Trabalho|O nome de exibição ou o GUID do trabalho|

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir assume a propriedade do trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /TakeOwnership myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)