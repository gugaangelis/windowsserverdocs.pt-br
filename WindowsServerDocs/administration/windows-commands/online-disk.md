---
title: disco online
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4c30d0853ff0ae065f02c0ee198c8cdcb90c950b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858057"
---
# <a name="online-disk"></a>disco online



Traz os discos que estão atualmente offline para um estado online.

> [!IMPORTANT]
> Esse comando não está disponível em nenhuma edição do Windows Vista.

> [!IMPORTANT]
> Esse comando irá falhar se for usado em um disco somente leitura.

Para obter instruções sobre como usar esse comando, consulte [reativar um disco ausente ou Offline dinâmica](https://go.microsoft.com/fwlink/?LinkId=207046) (https://go.microsoft.com/fwlink/?LinkId=207046).

## <a name="syntax"></a>Sintaxe

```
online disk [noerr]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|noerr|Somente para scripts. Quando um erro for encontrado, o DiskPart continua a processar comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro causar o DiskPart sair com um código de erro.|

## <a name="remarks"></a>Comentários

-   Quando usada sem parâmetros no Windows Vista, esse comando funciona em um grupo de discos. Para discos básicos, nunca há mais de um disco por grupo. Para discos dinâmicos, o grupo inclui todos os discos dinâmicos não-externo.
-   Para discos básicos, esse comando tentará colocar online o disco selecionado e todos os volumes nesse disco.
-   Para discos dinâmicos, esse comando tentará colocar online todos os discos que não são marcados como externos no computador local. Ele também tenta colocar online todos os volumes no conjunto de discos dinâmicos.
-   Se um disco dinâmico em um grupo de disco está online e é o único disco no grupo, em seguida, o grupo original é recriado e o disco é movido para esse grupo. Se houver outros discos no grupo e elas estejam online, o disco é simplesmente adicionado volta para o grupo.
-   Se o grupo de um disco selecionado contém volumes espelhados ou RAID-5, esse comando também sincroniza novamente esses volumes.
-   Um disco deve ser selecionado para este comando ter êxito. Use o **Selecionar disco** comando para selecionar um disco e mudar o foco a ele.

## <a name="BKMK_examples"></a>Exemplos

Para colocar o disco com foco on-line, digite:
```
online disk
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)

