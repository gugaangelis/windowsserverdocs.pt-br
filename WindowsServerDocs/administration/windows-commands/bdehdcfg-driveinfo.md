---
title: BdeHdCfg DriveInfo
description: Tópico de referência para o comando BdeHdCfg DriveInfo, que exibe a letra da unidade, o tamanho total, o máximo de espaço livre e as características da partição.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f2d065e7-eced-4509-a1a0-ee2521a7f02e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b18b4c3e128cd17353d369b418a049d0208cb654
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718684"
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
