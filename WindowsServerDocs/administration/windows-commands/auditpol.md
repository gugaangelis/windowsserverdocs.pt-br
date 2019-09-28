---
title: auditpol
description: Os comandos do Windows tópico para **Auditpol** – exibe informações sobre e executa funções para manipular políticas de auditoria.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a02cfb9d-732f-4e77-aeba-f18265daa3af
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5e249a9e2a07505f052b774208c514b4d16879b8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382385"
---
# <a name="auditpol"></a>auditpol



Exibe informações sobre e executa funções para manipular políticas de auditoria.

Para obter exemplos de como esse comando pode ser usado, consulte a seção exemplos em cada tópico.

## <a name="syntax"></a>Sintaxe

```
Auditpol command [<sub-command><options>]
```

## <a name="parameters"></a>Parâmetros

|Subcomando|Descrição|
|-----------|-----------|
|/Get|Exibe a política de auditoria atual.</br>Consulte [Auditpol Get](auditpol-get.md) para obter a sintaxe e as opções.|
|/Set|Define a política de auditoria.</br>Consulte [Auditpol set](auditpol-set.md) para obter a sintaxe e as opções.|
|/List|Exibe elementos de política selecionáveis.</br>Consulte a [lista Auditpol](auditpol-list.md) para ver a sintaxe e as opções.|
|/backup|Salva a política de auditoria em um arquivo.</br>Consulte [backup do Auditpol](auditpol-backup.md) para obter a sintaxe e as opções.|
|/Restore|Restaura a política de auditoria de um arquivo criado anteriormente usando Auditpol/backup.</br>Consulte [restauração de Auditpol](auditpol-restore.md) para obter a sintaxe e as opções.|
|/Clear|Limpa a política de auditoria.</br>Confira [Auditpol Clear](auditpol-clear.md) para obter a sintaxe e as opções.|
|/remove|Remove todas as configurações de política de auditoria por usuário e desabilita todas as configurações de política de auditoria do sistema.</br>Consulte [remoção de Auditpol](auditpol-remove.md) para obter a sintaxe e as opções.|
|/resourceSACL|Configura as SACLs (listas de controle de acesso) do sistema de recursos globais.</br>Observação: Aplica-se somente ao Windows 7 e ao Windows Server 2008 R2.</br>Consulte [Auditpol resourceSACL](auditpol-resourcesacl.md).|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

A ferramenta de linha de comando de diretiva de auditoria pode ser usada para:
-   Definir e consultar uma política de auditoria do sistema.
-   Definir e consultar uma política de auditoria por usuário.
-   Definir e consultar opções de auditoria.
-   Defina e consulte o descritor de segurança usado para delegar acesso a uma política de auditoria.
-   Relatar ou fazer backup de uma política de auditoria em um arquivo de texto de valores separados por vírgulas (CSV).
-   Carregar uma política de auditoria de um arquivo de texto CSV.
-   Configure as SACLs de recursos globais.

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)