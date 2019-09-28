---
title: disco online
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bc44a783-eaa4-40ca-be01-5703b5bf4eb3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3d798bf34ec2f9d2f01b5470c4ec52f936674135
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372515"
---
# <a name="online-disk"></a>disco online



Coloca os discos que estão offline no momento para um estado online.

> [!IMPORTANT]
> Esse comando não está disponível em nenhuma edição do Windows Vista.

> [!IMPORTANT]
> Esse comando falhará se for usado em um disco somente leitura.

Para obter instruções sobre como usar esse comando, consulte [reativar um disco dinâmico ausente ou offline](https://go.microsoft.com/fwlink/?LinkId=207046) (https://go.microsoft.com/fwlink/?LinkId=207046).

## <a name="syntax"></a>Sintaxe

```
online disk [noerr]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|NOERR|Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro.|

## <a name="remarks"></a>Comentários

-   Quando usado sem parâmetros no Windows Vista, esse comando opera em um grupo de discos. Para discos básicos, nunca há mais de um disco por grupo. Para discos dinâmicos, o grupo inclui todos os discos dinâmicos não estrangeiros.
-   Para discos básicos, esse comando tentará colocar online o disco selecionado e todos os volumes nesse disco.
-   Para discos dinâmicos, esse comando tentará colocar online todos os discos que não estão marcados como estranhos no computador local. Ele também tentará colocar todos os volumes online no conjunto de discos dinâmicos.
-   Se um disco dinâmico em um grupo de discos for colocado online e for o único disco do grupo, o grupo original será recriado e o disco será movido para esse grupo. Se houver outros discos no grupo e eles estiverem online, o disco será simplesmente adicionado de volta ao grupo.
-   Se o grupo de um disco selecionado contiver volumes espelhados ou RAID-5, esse comando também ressincronizará esses volumes.
-   Um disco deve ser selecionado para que esse comando tenha sucesso. Use o comando **selecionar disco** para selecionar um disco e deslocar o foco para ele.

## <a name="BKMK_examples"></a>Disso

Para colocar o disco em foco online, digite:
```
online disk
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)

