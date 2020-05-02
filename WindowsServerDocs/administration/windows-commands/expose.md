---
title: Expo
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9b0a21cf-3bef-4ade-b8f1-ac42f9203947
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 500dc5cfcd5e2bba4cfbc3cb5ef81a9065ea53cf
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725674"
---
# <a name="expose"></a>Expo



Expõe uma cópia de sombra persistente como uma letra de unidade, um compartilhamento ou um ponto de montagem.



## <a name="syntax"></a>Sintaxe

```
expose <ShadowID> {<Drive:> | <Share> | <MountPoint>}
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Shadowid|Especifica a ID de sombra da cópia de sombra que você deseja expor.|
|\<Unidade: >|Expõe a cópia de sombra especificada como uma letra da unidade (por exemplo, P:).|
|\<> de compartilhamento|Expõe a cópia de sombra especificada em um compartilhamento (por exemplo \\ \\, *MachineName*\).|
|\<> de MountPoint|Expõe a cópia de sombra especificada para um ponto de montagem (por exemplo\), C:\shadowcopy.|

## <a name="remarks"></a>Comentários

-   Você pode usar um alias existente ou uma variável de ambiente no lugar de *shadowid*. Use **Adicionar** sem parâmetros para ver os aliases existentes.

## <a name="examples"></a>Exemplos

Para expor a cópia de sombra persistente associada à variável de ambiente VSS_SHADOW_1 como a unidade X, digite:
```
expose %vss_shadow_1% x:
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)