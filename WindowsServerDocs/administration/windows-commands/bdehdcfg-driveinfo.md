---
title: bdehdcfg driveinfo
description: 'Tópico de comandos do Windows para * * bdehdcfg: driveinfo * * - exibe a letra da unidade, o tamanho total, o espaço livre máximo e as características da partição.'
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f2d065e7-eced-4509-a1a0-ee2521a7f02e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b2dd62e34f8205e0b5d395ba759fff4b4937b0ad
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435047"
---
# <a name="bdehdcfg-driveinfo"></a>bdehdcfg: driveinfo

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe a letra da unidade, o tamanho total, o espaço livre máximo e as características da partição. Apenas as partições válidas são listadas. O espaço não alocado não será listado se já existirem quatro partições primárias ou estendidas. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).
## <a name="syntax"></a>Sintaxe
```
bdehdcfg -driveinfo <DriveLetter>
```
### <a name="parameters"></a>Parâmetros

|   Parâmetro   |                  Descrição                  |
|---------------|-----------------------------------------------|
| <DriveLetter> | Especifica uma letra de unidade seguida por dois-pontos. |

## <a name="remarks"></a>Comentários
O comando é apenas informativo e não faz nenhuma modificação para a unidade.
## <a name="BKMK_Examples"></a>Exemplo
O exemplo a seguir exibirá as informações de unidade da unidade C.
```
bdehdcfg  driveinfo C:
```
## <a name="additional-references"></a>Referências adicionais
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [bdehdcfg](bdehdcfg.md)
