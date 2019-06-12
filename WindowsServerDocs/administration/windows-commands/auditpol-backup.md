---
title: backup Auditpol
description: Tópico de comandos do Windows para **auditpol backup** -faz backup do sistema de auditoria configurações de política de configurações de política de auditoria por usuário para todos os usuários e todas as opções de auditoria para um arquivo de texto de valores separados por vírgulas (CSV).
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 7de5e6dc6d205b7e6749d38ac822e31a78788c6e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435215"
---
# <a name="auditpol-backup"></a>backup Auditpol

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Faz backup do sistema de auditoria configurações de política de configurações de política de auditoria por usuário para todos os usuários e todas as opções de auditoria para um arquivo de texto de valores separados por vírgulas (CSV).

## <a name="syntax"></a>Sintaxe
```
auditpol /backup /file:<filename>
```
## <a name="parameters"></a>Parâmetros

| Parâmetro |                                 Descrição                                 |
|-----------|-----------------------------------------------------------------------------|
|   /file   | Especifica o nome do arquivo para o qual a política de auditoria será feita backup. |
|    /?     |                    Exibe a ajuda no prompt de comando.                     |

## <a name="remarks"></a>Comentários
para operações de backup para a política por usuário e a diretiva do sistema, você deve escrever ou conjunto de permissões de controle total nesse objeto no descritor de segurança. Você também pode executar operações de backup que possui o **gerenciar o log de auditoria e segurança** direito de usuário (SeSecurityPrivilege). No entanto, esse direito permite acesso adicional que não é necessário para executar a operação de lista.
## <a name="BKMK_examples"></a>Exemplos
Para fazer backup de auditoria por usuário configurações de política para todos os usuários, sistema e auditoria configurações de política de auditoria de todas as opções em um arquivo de texto formatado em CSV chamado auditpolicy.csv, tipo:
```
auditpol /backup /file:C:\auditpolicy.csv 
```
> [!NOTE]
> Se nenhuma unidade for especificada, o diretório atual será usado.
> #### <a name="additional-references"></a>Referências adicionais
> [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
> [auditpol restauração](auditpol-restore.md)
