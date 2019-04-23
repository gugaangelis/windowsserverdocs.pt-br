---
title: expor
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9b0a21cf-3bef-4ade-b8f1-ac42f9203947
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 51cc744bc2b61862ed05ca2e7d0aaa8f70d38692
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886657"
---
# <a name="expose"></a>expor



Expõe uma cópia de sombra persistente como uma letra de unidade, compartilhamento ou ponto de montagem.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
expose <ShadowID> {<Drive:> | <Share> | <MountPoint>}
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|ShadowID|Especifica a ID de sombra da cópia de sombra que você deseja expor.|
|\<Drive:>|Expõe a cópia de sombra especificado como uma letra de unidade (por exemplo, p).|
|\<Compartilhamento >|Expõe a cópia de sombra especificada em um compartilhamento (por exemplo, \\ \\ *MachineName*\).|
|\<MountPoint>|Expõe a cópia de sombra especificada para um ponto de montagem (por exemplo, C:\shadowcopy\).|

## <a name="remarks"></a>Comentários

-   Você pode usar um alias existente ou uma variável de ambiente no lugar de *ShadowID*. Use **adicionar** sem parâmetros para ver os aliases existentes.

## <a name="BKMK_examples"></a>Exemplos

Para expor a cópia de sombra persistente associada com a variável de ambiente VSS_SHADOW_1 como unidade X, digite:
```
expose %vss_shadow_1% x:
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)