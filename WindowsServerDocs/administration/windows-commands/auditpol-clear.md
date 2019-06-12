---
title: auditpol clara
description: Tópico de comandos do Windows para **auditpol clara** -exclui a política de auditoria por usuário para todos os usuários, redefinições (desativa) de diretiva para todas as subcategorias e conjuntos de opções de todos os a auditoria como desabilitado de auditoria do sistema.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 86b56386ba9bed2486cdf8cdbb4486fcec6c6265
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435154"
---
# <a name="auditpol-clear"></a>auditpol clara

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exclui a política de auditoria por usuário para todos os usuários, redefinições (desativa) de diretiva para todas as subcategorias e conjuntos de opções de todos os a auditoria como desabilitado de auditoria do sistema.

## <a name="syntax"></a>Sintaxe
```
auditpol /clear [/y]
```
## <a name="parameters"></a>Parâmetros

| Parâmetro |                                   Descrição                                    |
|-----------|----------------------------------------------------------------------------------|
|    /y     | Suprime o prompt para confirmar se todas as configurações de política de auditoria devem ser limpo. |
|    /?     |                       Exibe a ajuda no prompt de comando.                       |

## <a name="remarks"></a>Comentários
para operações de limpeza para a política por usuário e a diretiva do sistema, você deve escrever ou conjunto de permissões de controle total nesse objeto no descritor de segurança. Você também pode executar a operação de limpeza que possui o **gerenciar o log de auditoria e segurança** direito de usuário (SeSecurityPrivilege). No entanto, esse direito permite acesso adicional que não é necessário para executar a operação de limpeza.
## <a name="BKMK_examples"></a>Exemplos
Para excluir a política de auditoria por usuário para todos os usuários, redefinição (desabilitar) a política de auditoria do sistema para todas as subcategorias e defina as configurações de política de auditoria a ser desabilitada, em um prompt de confirmação, digite:
```
auditpol /clear
```
Para excluir a política de auditoria por usuário para todos os usuários, redefinir as configurações de política de auditoria do sistema para todas as subcategorias e defina a auditoria todas as configurações de política como desabilitado, sem um prompt de confirmação, digite:
```
auditpol /clear /y
```
> [!NOTE]
> O exemplo anterior é útil ao usar um script para executar esta operação.
> #### <a name="additional-references"></a>Referências adicionais
> [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
