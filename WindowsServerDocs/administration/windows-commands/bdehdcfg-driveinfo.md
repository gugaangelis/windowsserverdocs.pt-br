---
title: bdehdcfg driveinfo
description: Artigo de referência do comando BdeHdCfg DriveInfo, que exibe a letra da unidade, o tamanho total, o máximo de espaço livre e as características da partição.
ms.topic: reference
ms.assetid: f2d065e7-eced-4509-a1a0-ee2521a7f02e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 016733e2a10bd942b04a77af3d8e01577d4fff86
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89031554"
---
# <a name="bdehdcfg-driveinfo"></a>BdeHdCfg: DriveInfo

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe a letra da unidade, o tamanho total, o máximo de espaço livre e as características da partição. Apenas as partições válidas são listadas. O espaço não alocado não será listado se já existirem quatro partições primárias ou estendidas.

>[!NOTE]
> Esse comando é apenas informativo e não faz nenhuma alteração na unidade.

## <a name="syntax"></a>Sintaxe

```
bdehdcfg -driveinfo <drive_letter>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| <drive_letter> | Especifica uma letra de unidade seguida por dois-pontos. |

## <a name="example"></a>Exemplo

Para exibir as informações da unidade da unidade C::

```
bdehdcfg  driveinfo C:
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
