---
title: Voltar
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 3243f13a4997824d9fff7c874ce26d56325fefa4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371454"
---
# <a name="revert"></a>Voltar



reverte um volume de volta para uma cópia de sombra especificada. Isso tem suporte apenas para cópias de sombra no contexto CLIENTACCESSIBLE. Essas cópias de sombra são persistentes e só podem ser feitas pelo provedor do sistema. Se usado sem parâmetros, **REVERT** exibe a ajuda no prompt de comando.

## <a name="syntax"></a>Sintaxe

```
revert <ShadowCopyID>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<ShadowCopyID >|Especifica a ID da cópia de sombra para a qual reverter o volume.|

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)