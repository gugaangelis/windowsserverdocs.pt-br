---
title: Vssadmin delete sombras
description: Uma descrição do comando vssadmin delete sombras.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 71119174c7fc5929eb4e1982183ba0aed7eb1735
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2018
ms.locfileid: "2081776"
---
# <a name="vssadmin-delete-shadows"></a>Vssadmin delete sombras

>Aplica-se a: 10 do Windows, Windows 8.1, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Exclui as cópias de sombra do volume especificado.

## <a name="syntax"></a>Sintaxe

```PowerShell
vssadmin delete shadows /for=<ForVolumeSpec> [/oldest | /all | /shadow=<ShadowID>] [/quiet]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---|---|
|/ for = \ < ForVolumeSpec >|Especifica a cópia de sombra do volume que será excluída.|
|/ mais antiga|Exclui somente a cópia de sombra mais antiga.|
|/All|Exclui todas as cópias de sombra do volume especificado.|
|/ shadow = \ < ShadowID >|Exclui a cópia de sombra especificada pelo ShadowID. Para obter a identificação da cópia de sombra, use o comando **vssadmin sombras de lista** . Ao inserir uma ID de cópia de sombra, use o seguinte formato, onde cada *X* representa um caractere hexadecimal:<br><br>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX|
|/ silencioso|Especifica que o comando não exibirá mensagens durante a execução.|

## <a name="remarks"></a>Comentários

Você só pode excluir as cópias de sombra com o tipo de acessíveis ao cliente.

## <a name="examples"></a>Exemplos

Para excluir a cópia de sombra mais antiga do volume C, digite este comando:

```PowerShell
vssadmin delete shadows /for=c: /oldest
```

## <a name="additional-references"></a>Referências adicionais

* [Chave de sintaxe de linha de comando](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [Vssadmin](vssadmin.md)
* [Vssadmin sombras de lista](vssadmin-list-shadows.md)