---
title: revert
description: Artigo de referência para * * * *-
ms.topic: article
ms.assetid: 75ad40e4-502a-401e-b11e-8b31e00424b5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 719d1be30fd1d2874231e50b5cecb1604712c8b2
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87883612"
---
# <a name="revert"></a>revert



Reverte um volume de volta para uma cópia de sombra especificada. Isso tem suporte apenas para cópias de sombra no contexto CLIENTACCESSIBLE. Essas cópias de sombra são persistentes e só podem ser feitas pelo provedor do sistema. Se usado sem parâmetros, **REVERT** exibe a ajuda no prompt de comando.

## <a name="syntax"></a>Sintaxe

```
revert <ShadowCopyID>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<ShadowCopyID>|Especifica a ID da cópia de sombra para a qual reverter o volume.|

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)