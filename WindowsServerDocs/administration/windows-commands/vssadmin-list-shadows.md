---
title: Vssadmin sombras de lista
description: Uma descrição da lista vssadmin sombreia comando.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 07da36da1473563c3236a4fafc3ceae06259981a
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2018
ms.locfileid: "2081867"
---
# <a name="vssadmin-list-shadows"></a>Vssadmin sombras de lista

>Aplica-se a: 10 do Windows, Windows 8.1, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Lista todas as cópias de sombra existente de um volume especificado. Se você usar esse comando sem parâmetros, ele exibe todas as cópias de sombra de volume no computador na ordem determinada pelo **Conjunto de cópia de sombra**.

## <a name="syntax"></a>Sintaxe

```PowerShell
vssadmin list shadows [/for=<ForVolumeSpec>] [/shadow=<ShadowID>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---|---|
|/ for = \ < ForVolumeSpec >|Especifica qual serão listadas para as cópias de sombra de volume.|
|/ shadow = \ < ShadowID >|Lista especificada por ShadowID a cópia de sombra. Para obter a identificação da cópia de sombra, use o comando **vssadmin sombras de lista** . Quando você digita uma ID de cópia de sombra, use o seguinte formato, onde cada *X* representa um caractere hexadecimal:<br><br>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX|

## <a name="additional-references"></a>Referências adicionais

* [Chave de sintaxe de linha de comando](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [Vssadmin](vssadmin.md)