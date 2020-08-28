---
title: 'secedit: validar'
description: Artigo de referência para * * * *-
ms.topic: reference
ms.assetid: 9fb06354-f55a-4ca4-9fbc-9a872eb9b9cf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 47c0ec78cb49da3ce256e7a74ce8be003b0db85b
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89027974"
---
# <a name="seceditvalidate"></a>secedit: validar



Valida as configurações de segurança armazenadas em um modelo de segurança (arquivo. inf).

## <a name="syntax"></a>Sintaxe

```
Secedit /validate <configuration file name>

```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Nome do arquivo de configuração|Obrigatórios.</br>Especifica o caminho e o nome do arquivo para o modelo de segurança que será validado.|

## <a name="remarks"></a>Comentários

A validação de modelos de segurança pode ajudá-lo se um estiver corrompido ou definido incorretamente.

Um modelo de segurança inválido não será aplicado.

O arquivo de log não será atualizado.

No Windows Server 2008, foi `Secedit /refreshpolicy` substituído por `gpupdate` . Para obter informações sobre como atualizar as configurações de segurança, consulte [gpupdate](gpupdate.md).

## <a name="examples"></a>Exemplos

Depois que uma reversão é executada em um modelo de segurança, você deseja verificar se o arquivo inf de reversão, secRBKcontoso. inf, é válido.
```
Secedit /validate secRBKcontoso.inf
```

## <a name="additional-references"></a>Referências adicionais

-   [Secedit:generaterollback](secedit-generaterollback.md)
-   [Utilitário](secedit.md)
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)