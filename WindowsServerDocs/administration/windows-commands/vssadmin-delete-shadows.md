---
title: Sombras de exclusão vssadmin
description: Uma descrição do comando vssadmin delete sombras.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 83074017e4ae412cf0aec654f6ab5901ad8039e2
ms.sourcegitcommit: 2977c707a299929c6ab0d1e0adab2e1c644b8306
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63706623"
---
# <a name="vssadmin-delete-shadows"></a>Sombras de exclusão vssadmin

>Aplica-se a: Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Exclui as cópias de sombra do volume especificado.

## <a name="syntax"></a>Sintaxe

```PowerShell
vssadmin delete shadows /for=<ForVolumeSpec> [/oldest | /all | /shadow=<ShadowID>] [/quiet]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---|---|
|/for=\<ForVolumeSpec>|Especifica a cópia de sombra do volume que será excluída.|
|/oldest|Exclui apenas a cópia de sombra mais antiga.|
|/all|Exclui todas as cópias de sombra do volume especificado.|
|/shadow=\<ShadowID>|Exclui a cópia de sombra especificada pelo ShadowID. Para obter a ID da cópia de sombra, use o **vssadmin lista sombras** comando. Quando você inserir uma ID de cópia de sombra, use o seguinte formato, onde cada *X* representa um caractere hexadecimal:<br><br>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX|
|/quiet|Especifica que o comando não exibirá mensagens durante a execução.|

## <a name="remarks"></a>Comentários

Você só pode excluir as cópias de sombra com o tipo acessíveis ao cliente.

## <a name="examples"></a>Exemplos

Para excluir a cópia de sombra mais antiga do volume C, digite este comando:

```PowerShell
vssadmin delete shadows /for=c: /oldest
```

## <a name="additional-references"></a>Referências adicionais

* [Chave de sintaxe de linha de comando](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [Vssadmin](vssadmin.md)
* [Vssadmin lista sombras](vssadmin-list-shadows.md)