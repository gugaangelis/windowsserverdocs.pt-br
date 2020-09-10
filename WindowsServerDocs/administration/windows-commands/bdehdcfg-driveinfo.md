---
title: bdehdcfg driveinfo
description: Artigo de referência do comando BdeHdCfg DriveInfo, que exibe a letra da unidade, o tamanho total, o máximo de espaço livre e as características da partição.
ms.topic: reference
ms.assetid: f2d065e7-eced-4509-a1a0-ee2521a7f02e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: fb474e40e92979f5f2cf73d90a553bbf785c0312
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632959"
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
