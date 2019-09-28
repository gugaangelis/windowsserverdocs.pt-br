---
title: Expo
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 819484364e8375c4d58e4d022681eedeaa7084ab
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377283"
---
# <a name="expose"></a>Expo



expõe uma cópia de sombra persistente como uma letra de unidade, um compartilhamento ou um ponto de montagem.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
expose <ShadowID> {<Drive:> | <Share> | <MountPoint>}
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Shadowid|Especifica a ID de sombra da cópia de sombra que você deseja expor.|
|\<Drive: >|Expõe a cópia de sombra especificada como uma letra da unidade (por exemplo, P:).|
|\<Share >|Expõe a cópia de sombra especificada em um compartilhamento (por exemplo, \\ @ no__t-1*MachineName*\).|
|\<MountPoint >|Expõe a cópia de sombra especificada para um ponto de montagem (por exemplo, C:\shadowcopy @ no__t-0.|

## <a name="remarks"></a>Comentários

-   Você pode usar um alias existente ou uma variável de ambiente no lugar de *shadowid*. Use **Adicionar** sem parâmetros para ver os aliases existentes.

## <a name="BKMK_examples"></a>Disso

Para expor a cópia de sombra persistente associada à variável de ambiente VSS_SHADOW_1 como a unidade X, digite:
```
expose %vss_shadow_1% x:
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)