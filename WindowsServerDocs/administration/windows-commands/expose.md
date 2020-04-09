---
title: Expo
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9b0a21cf-3bef-4ade-b8f1-ac42f9203947
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 55cc7a292b81977a346f3f078a3b5623243ea46c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844799"
---
# <a name="expose"></a>Expo



expõe uma cópia de sombra persistente como uma letra de unidade, um compartilhamento ou um ponto de montagem.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
expose <ShadowID> {<Drive:> | <Share> | <MountPoint>}
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Shadowid|Especifica a ID de sombra da cópia de sombra que você deseja expor.|
|Unidade de \<: >|Expõe a cópia de sombra especificada como uma letra da unidade (por exemplo, P:).|
|> de \<compartilhamento|Expõe a cópia de sombra especificada em um compartilhamento (por exemplo, \\\\*MachineName*\).|
|> de \<MountPoint|Expõe a cópia de sombra especificada para um ponto de montagem (por exemplo, C:\shadowcopy\).|

## <a name="remarks"></a>Comentários

-   Você pode usar um alias existente ou uma variável de ambiente no lugar de *shadowid*. Use **Adicionar** sem parâmetros para ver os aliases existentes.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para expor a cópia de sombra persistente associada à variável de ambiente VSS_SHADOW_1 como a unidade X, digite:
```
expose %vss_shadow_1% x:
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)