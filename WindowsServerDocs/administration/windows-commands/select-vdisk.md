---
title: selecionar vdisk
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8808872a-3523-4205-a6c6-83fa738ee37a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 23741d9211ebdf98ac198af2ae1c562724a1955b
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2020
ms.locfileid: "83821076"
---
# <a name="select-vdisk"></a>selecionar vdisk

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

-   [Compact vdisk](compact-vdisk.md)



-   [Desanexar vdisk](detach-vdisk.md)

-   [detalhes do VDISK](detail-vdisk.md)

-   [expandir vdisk](expand-vdisk.md)

-   [Mesclar vdisk](merge-vdisk.md)

-   [list_1](list_1.md)


