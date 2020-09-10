---
title: select vdisk
description: Artigo de referência para * * * *-
ms.topic: reference
ms.assetid: 8808872a-3523-4205-a6c6-83fa738ee37a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: daa6d3047e28fb8f6084824fdf1557423daa52b8
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639145"
---
# <a name="select-vdisk"></a>select vdisk

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

seleciona o VHD do disco rígido virtual especificado \( \) e desloca o foco para ele.

> [!NOTE]
> Esse comando só é aplicável ao Windows 7 e ao Windows Server 2008 R2.

## <a name="syntax"></a>Sintaxe

```
select vdisk file=<full path> [noerr]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------|--------|
|Grupo\=<full path>|Especifica o caminho completo e o nome de arquivo de um arquivo VHD existente.|
|NOERR|Usado somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro.|

## <a name="examples"></a>Exemplos
Para deslocar o foco para o VHD chamado Test. VHD, digite:

```
select vdisk file=c:\test\test.vhd
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

-   [attach vdisk](attach-vdisk.md)

-   [compact vdisk](compact-vdisk.md)



-   [Desanexar vdisk](detach-vdisk.md)

-   [detail vdisk](detail-vdisk.md)

-   [expand vdisk](expand-vdisk.md)

-   [Mesclar vdisk](merge-vdisk.md)

-   [list_1](./list.md)