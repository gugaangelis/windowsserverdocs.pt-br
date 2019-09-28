---
title: Auditpol limpo
description: Os comandos do Windows tópico para **Auditpol Clear** – exclui a política de auditoria por usuário para todos os usuários, redefine (desabilita) a política de auditoria do sistema para todas as subcategorias e define todas as opções de auditoria como desabilitadas.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 05bfa218-2434-4ad1-b33c-e6fcfb2b4f67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4fd2cce2b860ee41725b698dcd36ca38b2c4c6a8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382421"
---
# <a name="auditpol-clear"></a>Auditpol limpo

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

exclui a política de auditoria por usuário para todos os usuários, redefine (desabilita) a política de auditoria do sistema para todas as subcategorias e define todas as opções de auditoria como desabilitadas.

## <a name="syntax"></a>Sintaxe
```
auditpol /clear [/y]
```
## <a name="parameters"></a>Parâmetros

| Parâmetro |                                   Descrição                                    |
|-----------|----------------------------------------------------------------------------------|
|    /y     | Suprime o prompt para confirmar se todas as configurações de política de auditoria devem ser limpas. |
|    /?     |                       Exibe a ajuda no prompt de comando.                       |

## <a name="remarks"></a>Comentários
para operações claras para a política por usuário e a política do sistema, você deve ter a permissão gravar ou controle total nesse objeto definido no descritor de segurança. Você também pode executar a operação de limpeza por meio do direito de usuário **gerenciar auditoria e log de segurança** (SeSecurityPrivilege). No entanto, esse direito permite o acesso adicional que não é necessário para executar a operação de limpeza.
## <a name="BKMK_examples"></a>Disso
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
> #### <a name="additional-references"></a>Referências adicionais
> [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
