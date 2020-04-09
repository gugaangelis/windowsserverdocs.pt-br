---
title: Auditpol limpo
description: O tópico de comandos do Windows para **Auditpol Clear**, que exclui a política de auditoria por usuário para todos os usuários, redefine (desabilita) a política de auditoria do sistema para todas as subcategorias e define todas as opções de auditoria como desabilitadas.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 05bfa218-2434-4ad1-b33c-e6fcfb2b4f67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 971f4ba54d787be29cb9e7d710f556c50c69a8dc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851199"
---
# <a name="auditpol-clear"></a>Auditpol limpo

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exclui a política de auditoria por usuário para todos os usuários, redefine (desabilita) a política de auditoria do sistema para todas as subcategorias e define todas as opções de auditoria como desabilitadas.

## <a name="syntax"></a>Sintaxe

```
auditpol /clear [/y]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| ----------- | --------------- |
| /y | Suprime o prompt para confirmar se todas as configurações de política de auditoria devem ser limpas. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="remarks"></a>Comentários

Para operações claras para a política por usuário e a política do sistema, você deve ter a permissão gravar ou controle total nesse objeto definido no descritor de segurança. Você também pode executar a operação de limpeza por meio do direito de usuário **gerenciar auditoria e log de segurança** (SeSecurityPrivilege). No entanto, esse direito permite o acesso adicional que não é necessário para executar a operação de limpeza.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

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
