---
title: Vssadmin lista sombras
description: Uma descrição da vssadmin lista sombras comando.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 3601986a51e8c5b362a28c686ed132eda8e4b640
ms.sourcegitcommit: 2977c707a299929c6ab0d1e0adab2e1c644b8306
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63706559"
---
# <a name="vssadmin-list-shadows"></a>Vssadmin lista sombras

>Aplica-se a: Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Lista todas as cópias de sombra existentes de um volume especificado. Se você usar esse comando sem parâmetros, ele exibe todas as cópias de sombra de volume no computador na ordem determinada pelas **conjunto de cópias de sombra**.

## <a name="syntax"></a>Sintaxe

```PowerShell
vssadmin list shadows [/for=<ForVolumeSpec>] [/shadow=<ShadowID>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---|---|
|/for=\<ForVolumeSpec>|Especifica qual serão listadas para as cópias de sombra de volume.|
|/shadow=\<ShadowID>|Lista a cópia de sombra especificada pelo ShadowID. Para obter a ID da cópia de sombra, use o **vssadmin lista sombras** comando. Quando você digitar uma ID de cópia de sombra, use o seguinte formato, onde cada *X* representa um caractere hexadecimal:<br><br>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX|

## <a name="additional-references"></a>Referências adicionais

* [Chave de sintaxe de linha de comando](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [Vssadmin](vssadmin.md)