---
title: 'secedit: validar'
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9fb06354-f55a-4ca4-9fbc-9a872eb9b9cf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b9425f7a1fb821f4ecbaa7c1689c3baabbff6223
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834869"
---
# <a name="seceditvalidate"></a>secedit: validar



Valida as configurações de segurança armazenadas em um modelo de segurança (arquivo. inf). Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
Secedit /validate <configuration file name>  

```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Nome do arquivo de configuração|Obrigatório.</br>Especifica o caminho e o nome do arquivo para o modelo de segurança que será validado.|

## <a name="remarks"></a>Comentários

A validação de modelos de segurança pode ajudá-lo se um estiver corrompido ou definido incorretamente.

Um modelo de segurança inválido não será aplicado.

O arquivo de log não será atualizado.

No Windows Server 2008, `Secedit /refreshpolicy` foi substituído por `gpupdate`. Para obter informações sobre como atualizar as configurações de segurança, consulte [gpupdate](gpupdate.md).

## <a name="examples"></a><a name=BKMK_Examples></a>Disso

Depois que uma reversão é executada em um modelo de segurança, você deseja verificar se o arquivo inf de reversão, secRBKcontoso. inf, é válido.
```
Secedit /validate secRBKcontoso.inf
```

## <a name="additional-references"></a>Referências adicionais

-   [Secedit:generaterollback](secedit-generaterollback.md)
-   [Utilitário](secedit.md)
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)