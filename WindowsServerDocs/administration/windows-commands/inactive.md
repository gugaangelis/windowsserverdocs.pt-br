---
title: inativo
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f4fb4695-4e66-4166-b4ab-2c86a4605580
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9c8ded732d984830c7892720f75938979f1abb67
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438158"
---
# <a name="inactive"></a>inativo



No mestre de inicialização básica discos MBR (registro), marca a partição do sistema ou a partição de inicialização com foco como inativa.

## <a name="syntax"></a>Sintaxe

```
inactive
```

## <a name="remarks"></a>Comentários

> [!CAUTION]
> O computador talvez não seja iniciado sem uma partição ativa. Não marca uma partição de sistema ou de inicialização como inativa, a menos que você for um usuário experiente com uma compreensão completa da família de sistemas operacionais do Windows.</br>> Se você não conseguir iniciar o computador depois de marcar a partição de sistema ou de inicialização como inativa, insira o CD de instalação do Windows na unidade de CD-ROM, reinicie o computador e, em seguida, repare a partição usando o **fixmbr** e **fixboot** comandos no Console de recuperação.
> -   Depois de marcar a partição do sistema ou a partição de inicialização como inativo, o computador é iniciado na próxima opção especificada no BIOS, como a unidade de CD-ROM ou um Pre-Boot eXecution Environment (PXE).
> -   Uma partição ativa de sistema ou de inicialização deve ser selecionada para essa operação seja bem-sucedida. Use o **Selecionar partição** comando para selecionar a partição ativa e mudar o foco a ele.

## <a name="BKMK_examples"></a>Exemplos

```
inactive
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)

