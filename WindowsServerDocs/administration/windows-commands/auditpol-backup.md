---
title: backup de Auditpol
description: Tópico de comandos do Windows para **backup do Auditpol** – faz backup das configurações da política de auditoria do sistema, configurações de política de auditoria por usuário para todos os usuários e todas as opções de auditoria para um arquivo de texto de valores separados por vírgulas (CSV).
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc84e581-aa0f-4c91-b13b-1d970bad5517
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 96b98a05740d3ce1bfe14eda4c5d97ba6c09ff32
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382455"
---
# <a name="auditpol-backup"></a>backup de Auditpol

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Faz backup das configurações da política de auditoria do sistema, das configurações de política de auditoria por usuário para todos os usuários e de todas as opções de auditoria para um arquivo de texto de valores separados por vírgulas (CSV).

## <a name="syntax"></a>Sintaxe
```
auditpol /backup /file:<filename>
```
## <a name="parameters"></a>Parâmetros

| Parâmetro |                                 Descrição                                 |
|-----------|-----------------------------------------------------------------------------|
|   /File   | Especifica o nome do arquivo para o qual será feito o backup da diretiva de auditoria. |
|    /?     |                    Exibe a ajuda no prompt de comando.                     |

## <a name="remarks"></a>Comentários
para operações de backup para a política por usuário e a política do sistema, você deve ter a permissão gravar ou controle total nesse objeto definido no descritor de segurança. Você também pode executar operações de backup por meio do direito de usuário **gerenciar auditoria e log de segurança** (SeSecurityPrivilege). No entanto, esse direito permite o acesso adicional que não é necessário para executar a operação de lista.
## <a name="BKMK_examples"></a>Disso
Para fazer backup de configurações de política de auditoria por usuário para todos os usuários, configurações de política de auditoria do sistema e todas as opções de auditoria em um arquivo de texto formatado para CSV chamado Auditpolicy. csv, digite:
```
auditpol /backup /file:C:\auditpolicy.csv 
```
> [!NOTE]
> Se nenhuma unidade for especificada, o diretório atual será usado.
> #### <a name="additional-references"></a>referências adicionais
> [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
> [restauração de Auditpol](auditpol-restore.md)
