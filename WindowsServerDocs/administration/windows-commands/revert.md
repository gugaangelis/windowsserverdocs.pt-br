---
title: Reverter
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 75ad40e4-502a-401e-b11e-8b31e00424b5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5bc77b17317f602d642c7a9e025b67be10ad7256
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875107"
---
# <a name="revert"></a>Reverter



Reverte o volume de volta para uma cópia de sombra especificado. Isso é suportado apenas para cópias de sombra no contexto CLIENTACCESSIBLE. Essas cópias de sombra são persistentes e só podem ser feitas pelo provedor de sistema. Se usado sem parâmetros, **reverter** exibe a Ajuda no prompt de comando.

## <a name="syntax"></a>Sintaxe

```
revert <ShadowCopyID>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<ShadowCopyID>|Especifica a ID da cópia de sombra para reverter o volume.|

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)