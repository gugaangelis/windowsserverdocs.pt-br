---
title: auditpol
description: Tópico de comandos do Windows para **auditpol** - exibe informações sobre e executa funções para manipular as políticas de auditoria.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d7e8364be977e868ac161704e67c37ec5c400e49
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849217"
---
# <a name="auditpol"></a>auditpol



Exibe informações sobre e executa funções para manipular as políticas de auditoria.

Para obter exemplos de como esse comando pode ser usado, consulte a seção de exemplos em cada tópico.

## <a name="syntax"></a>Sintaxe

```
Auditpol command [<sub-command><options>]
```

## <a name="parameters"></a>Parâmetros

|Subcomando|Descrição|
|-----------|-----------|
|/get|Exibe a atual política de auditoria.</br>Ver [Auditpol get](auditpol-get.md) para sintaxe e opções.|
|/set|Define a política de auditoria.</br>Ver [Auditpol set](auditpol-set.md) para sintaxe e opções.|
|/list|Exibe os elementos da diretiva selecionáveis.</br>Ver [Auditpol list](auditpol-list.md) para sintaxe e opções.|
|/ backup|Salva a política de auditoria em um arquivo.</br>Ver [Auditpol backup](auditpol-backup.md) para sintaxe e opções.|
|/restore|Restaura a política de auditoria de um arquivo que foi criado anteriormente usando auditpol /backup.</br>Ver [restauração Auditpol](auditpol-restore.md) para sintaxe e opções.|
|/clear|Limpa a política de auditoria.</br>Ver [Auditpol clara](auditpol-clear.md) para sintaxe e opções.|
|/remove|Remove todas as configurações de política de auditoria por usuário e desabilita todas as configurações de diretiva de auditoria de sistema.</br>Ver [Auditpol remover](auditpol-remove.md) para sintaxe e opções.|
|/resourceSACL|Configura as listas de controle de acesso do recurso global sistema (SACL).</br>Observação: Aplica-se somente ao Windows 7 e Windows Server 2008 R2.</br>Ver [Auditpol resourceSACL](auditpol-resourcesacl.md).|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

A ferramenta de linha de comando de diretiva de auditoria pode ser usada para:
-   Definir e consultar uma política de auditoria do sistema.
-   Definir e consultar uma política de auditoria por usuário.
-   Definir e consultar as opções de auditoria.
-   Definir e consultar o descritor de segurança usado para delegar acesso a uma política de auditoria.
-   Relatar ou fazer backup de uma política de auditoria em um arquivo de texto de valores separados por vírgulas (CSV).
-   Carrega uma política de auditoria de um arquivo CSV.
-   Configure SACLs de recursos globais.

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)