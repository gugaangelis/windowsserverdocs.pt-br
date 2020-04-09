---
title: BdeHdCfg DriveInfo
description: O tópico de comandos do Windows para **BdeHdCfg DriveInfo**, que exibe a letra da unidade, o tamanho total, o espaço livre máximo e as características da partição.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f2d065e7-eced-4509-a1a0-ee2521a7f02e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9c598ea2d1d140090d623b3b48dbcc1be51ee66c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851059"
---
# <a name="bdehdcfg-driveinfo"></a>BdeHdCfg: DriveInfo

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe a letra da unidade, o tamanho total, o máximo de espaço livre e as características da partição. Apenas as partições válidas são listadas. O espaço não alocado não será listado se já existirem quatro partições primárias ou estendidas.

Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
bdehdcfg -driveinfo <DriveLetter>
```

#### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| <DriveLetter> | Especifica uma letra de unidade seguida por dois-pontos. |

## <a name="remarks"></a>Comentários

O comando é apenas informativo e não faz nenhuma modificação na unidade.

## <a name="example"></a><a name=BKMK_Examples></a>Exemplo

O exemplo a seguir exibirá as informações da unidade para a unidade C.

```
bdehdcfg  driveinfo C:
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
