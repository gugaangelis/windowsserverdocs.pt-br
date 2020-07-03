---
title: auditpol backup
description: Artigo de referência para o comando de backup Auditpol, que faz o backup das configurações da política de auditoria do sistema, das configurações de política de auditoria por usuário para todos os usuários e de todas as opções de auditoria para um arquivo de texto CSV (valores separados por vírgula).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dc84e581-aa0f-4c91-b13b-1d970bad5517
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8678dbc80b664e3ab667f197f708fbbdbbe40dc7
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923810"
---
# <a name="auditpol-backup"></a>auditpol backup

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Faz backup das configurações da política de auditoria do sistema, das configurações de política de auditoria por usuário para todos os usuários e de todas as opções de auditoria para um arquivo de texto de valores separados por vírgulas (CSV).

Para executar operações de *backup* nas políticas *por usuário* e *sistema* , você deve ter a permissão **gravar** ou **controle total** para esse objeto definido no descritor de segurança. Você também pode executar operações de *backup* se tiver o direito de usuário **gerenciar auditoria e log de segurança** (SeSecurityPrivilege). No entanto, esse direito permite o acesso adicional que não é necessário para executar as operações de *backup* gerais.

## <a name="syntax"></a>Sintaxe

```
auditpol /backup /file:<filename>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|-----------|------------- |
| /File | Especifica o nome do arquivo para o qual será feito o backup da diretiva de auditoria. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para fazer backup de configurações de política de auditoria por usuário para todos os usuários, configurações de política de auditoria do sistema e todas as opções de auditoria em um arquivo de texto formatado para CSV chamado auditpolicy.csv, digite:

```
auditpol /backup /file:C:\auditpolicy.csv
```

> [!NOTE]
> Se nenhuma unidade for especificada, o diretório atual será usado.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [auditpol restore](auditpol-restore.md)

- [comandos Auditpol](auditpol.md)
