---
title: Vssadmin excluir sombras
description: Uma descrição do comando VSSadmin Delete Shadows.
ms.topic: reference
author: JasonGerend
ms.author: jgerend
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 667aaa7477666c6128aaed4ddb10a9f3695e571a
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89022860"
---
# <a name="vssadmin-delete-shadows"></a>Vssadmin excluir sombras

> Aplica-se a: Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Exclui as cópias de sombra de um volume especificado.

## <a name="syntax"></a>Sintaxe

```PowerShell
vssadmin delete shadows /for=<ForVolumeSpec> [/oldest | /all | /shadow=<ShadowID>] [/quiet]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---|---|
|/for =\<ForVolumeSpec>|Especifica qual cópia de sombra do volume será excluída.|
|/oldest|Exclui somente a cópia de sombra mais antiga.|
|/all|Exclui todas as cópias de sombra do volume especificado.|
|/Shadow =\<ShadowID>|Exclui a cópia de sombra especificada por Shadowid. Para obter a ID da cópia de sombra, use o comando **VSSadmin list Shadows** . Ao inserir uma ID de cópia de sombra, use o seguinte formato, em que cada *X* representa um caractere hexadecimal:<br><br>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX|
|/quiet|Especifica que o comando não exibirá mensagens durante a execução.|

## <a name="remarks"></a>Comentários

Você só pode excluir cópias de sombra com o tipo acessível pelo cliente.

## <a name="examples"></a>Exemplos

Para excluir a cópia de sombra mais antiga do volume C, digite este comando:

```PowerShell
vssadmin delete shadows /for=c: /oldest
```

## <a name="additional-references"></a>Referências adicionais

* [Chave de sintaxe de linha de comando](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [Vssadmin](vssadmin.md)
* [Vssadmin da lista de sombras](vssadmin-list-shadows.md)
