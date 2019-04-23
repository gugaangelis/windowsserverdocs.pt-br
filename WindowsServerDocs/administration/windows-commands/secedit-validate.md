---
title: 'Secedit: validar'
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: cca64f6b2904ed11f6b45e316c8e4da0093c373e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877907"
---
# <a name="seceditvalidate"></a>Secedit: validar



Valida as configurações de segurança armazenadas em um modelo de segurança (arquivo. inf). Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
Secedit /validate <configuration file name>  

```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Nome do arquivo de configuração|Obrigatório.</br>Especifica o nome de arquivo e caminho para o modelo de segurança que será validado.|

## <a name="remarks"></a>Comentários

Validar modelos de segurança pode ajudá-lo a se uma está corrompida ou foi definida inadequadamente.

Um modelo de segurança inválidas não será aplicado.

Não será possível atualizar o arquivo de log.

No Windows Server 2008, `Secedit /refreshpolicy` foi substituído por `gpupdate`. Para obter informações sobre como atualizar as configurações de segurança, consulte [Gpupdate](gpupdate.md).

## <a name="BKMK_Examples"></a>Exemplos

Depois de uma reversão for executada em um modelo de segurança, você deseja verificar se o arquivo inf de reversão, secRBKcontoso.inf, é válido.
```
Secedit /validate secRBKcontoso.inf
```

#### <a name="additional-references"></a>Referências adicionais

-   [Secedit:generaterollback](secedit-generaterollback.md)
-   [Secedit](secedit.md)
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)