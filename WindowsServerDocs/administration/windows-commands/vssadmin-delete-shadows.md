---
title: Vssadmin excluir sombras
description: Uma descrição do comando VSSadmin Delete Shadows.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: b52701f831502c16487dbbaf0cd61d839c046c68
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436701"
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
|/for = \< ForVolumeSpec>|Especifica qual cópia de sombra do volume será excluída.|
|/oldest|Exclui somente a cópia de sombra mais antiga.|
|/all|Exclui todas as cópias de sombra do volume especificado.|
|/Shadow = \< shadowid>|Exclui a cópia de sombra especificada por Shadowid. Para obter a ID da cópia de sombra, use o comando **VSSadmin list Shadows** . Ao inserir uma ID de cópia de sombra, use o seguinte formato, em que cada *X* representa um caractere hexadecimal:<br><br>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX|
|/quiet|Especifica que o comando não exibirá mensagens durante a execução.|

## <a name="remarks"></a>Comentários

Você só pode excluir cópias de sombra com o tipo acessível pelo cliente.

## <a name="examples"></a>Exemplos

Para excluir a cópia de sombra mais antiga do volume C, digite este comando:

```PowerShell
vssadmin delete shadows /for=c: /oldest
```

## <a name="additional-references"></a>Referências adicionais

* [Chave de sintaxe de linha de comando](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [Vssadmin](vssadmin.md)
* [Vssadmin da lista de sombras](vssadmin-list-shadows.md)