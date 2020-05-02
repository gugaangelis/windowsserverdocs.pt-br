---
title: Auditpol limpo
description: Tópico de referência para o comando limpar Auditpol, que exclui a política de auditoria por usuário para todos os usuários, redefine (desabilita) a política de auditoria do sistema para todas as subcategorias e define todas as opções de auditoria como desabilitadas.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 05bfa218-2434-4ad1-b33c-e6fcfb2b4f67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a3d4765907f1dd614f5d0a61585ea09069652ecb
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719144"
---
# <a name="auditpol-clear"></a>Auditpol limpo

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exclui a política de auditoria por usuário para todos os usuários, redefine (desabilita) a política de auditoria do sistema para todas as subcategorias e define todas as opções de auditoria como desabilitadas.

Para executar operações *claras* nas políticas *por usuário* e *sistema* , você deve ter a permissão **gravar** ou **controle total** para esse objeto definido no descritor de segurança. Você também pode executar operações *claras* se tiver o direito de usuário **gerenciar auditoria e log de segurança** (SeSecurityPrivilege). No entanto, esse direito permite acesso adicional que não é necessário para executar as operações de *limpeza* geral.

## <a name="syntax"></a>Sintaxe

```
auditpol /clear [/y]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| ----------- | --------------- |
| /y | Suprime o prompt para confirmar se todas as configurações de política de auditoria devem ser limpas. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para excluir a política de auditoria por usuário para todos os usuários, redefina (desabilite) a política de auditoria do sistema para todas as subcategorias e defina todas as configurações de política de auditoria como desabilitadas, em um prompt de confirmação, digite:

```
auditpol /clear
```

Para excluir a política de auditoria por usuário para todos os usuários, redefina as configurações da política de auditoria do sistema para todas as subcategorias e defina todas as configurações da política de auditoria como desabilitado, sem um prompt de confirmação, digite:

```
auditpol /clear /y
```

> [!NOTE]
> O exemplo anterior é útil ao usar um script para executar essa operação.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comandos Auditpol](auditpol.md)
