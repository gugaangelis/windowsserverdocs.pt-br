---
title: 'secedit: validar'
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9fb06354-f55a-4ca4-9fbc-9a872eb9b9cf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ece0a0324b77eb4226b679bc29f7bd599f15a120
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371097"
---
# <a name="seceditvalidate"></a>secedit: validar



Valida as configurações de segurança armazenadas em um modelo de segurança (arquivo. inf). Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
Secedit /validate <configuration file name>  

```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Nome do arquivo de configuração|Obrigatório.</br>Especifica o caminho e o nome do arquivo para o modelo de segurança que será validado.|

## <a name="remarks"></a>Comentários

A validação de modelos de segurança pode ajudá-lo se um estiver corrompido ou definido incorretamente.

Um modelo de segurança inválido não será aplicado.

O arquivo de log não será atualizado.

No Windows Server 2008, `Secedit /refreshpolicy` foi substituído por. `gpupdate` Para obter informações sobre como atualizar as configurações de segurança, consulte [gpupdate](gpupdate.md).

## <a name="BKMK_Examples"></a>Disso

Depois que uma reversão é executada em um modelo de segurança, você deseja verificar se o arquivo inf de reversão, secRBKcontoso. inf, é válido.
```
Secedit /validate secRBKcontoso.inf
```

#### <a name="additional-references"></a>Referências adicionais

-   [Secedit:generaterollback](secedit-generaterollback.md)
-   [Utilitário](secedit.md)
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)