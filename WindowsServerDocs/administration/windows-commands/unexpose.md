---
title: Cancele
description: O tópico de comandos do Windows para o unexpo, que não expõe uma cópia de sombra que foi exposta usando o comando Expose.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 58dc7d0f-52e9-4587-9487-d3b4c3e52640
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f2f8bbdb3b810ffbf9332608a016fc3b3e188e9f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832349"
---
# <a name="unexpose"></a>Cancele

Não expõe uma cópia de sombra que foi exposta usando o comando **Expo** . A cópia de sombra exposta pode ser especificada por sua ID de sombra, letra de unidade, compartilhamento ou ponto de montagem.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
unexpose {<ShadowID> | <Drive:> | <Share> | <MountPoint>}
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Shadowid >|Revela a cópia de sombra especificada pela ID de sombra fornecida.|
|Unidade de \<: >|Não expõe a cópia de sombra associada à letra da unidade especificada (por exemplo, a unidade P).|
|> de \<compartilhamento|Revela a cópia de sombra associada ao compartilhamento especificado (por exemplo, \\\\*MachineName*\).|
|> de \<MountPoint|Não expõe a cópia de sombra associada ao ponto de montagem especificado (por exemplo, C:\shadowcopy\).|

## <a name="remarks"></a>Comentários

-   Você pode usar um alias existente ou uma variável de ambiente no lugar de *shadowid*. Use **Adicionar** sem parâmetros para ver os aliases existentes.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para revelar a cópia de sombra associada à unidade P, digite:
```
unexpose P:
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)