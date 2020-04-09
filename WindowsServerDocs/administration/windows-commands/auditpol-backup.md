---
title: backup de Auditpol
description: Tópico de comandos do Windows para o **backup do Auditpol**, que faz backup das configurações da política de auditoria do sistema, das configurações de política de auditoria por usuário para todos os usuários e de todas as opções de auditoria para um arquivo de texto CSV (valores separados por vírgula).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dc84e581-aa0f-4c91-b13b-1d970bad5517
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8895f312606a6a6c45a77c659a1cd98d115babe3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851209"
---
# <a name="auditpol-backup"></a>backup de Auditpol

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Faz backup das configurações da política de auditoria do sistema, das configurações de política de auditoria por usuário para todos os usuários e de todas as opções de auditoria para um arquivo de texto de valores separados por vírgulas (CSV).

## <a name="syntax"></a>Sintaxe

```
auditpol /backup /file:<filename>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|-----------|------------- |
| /File | Especifica o nome do arquivo para o qual será feito o backup da diretiva de auditoria. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="remarks"></a>Comentários

Para operações de backup para a política por usuário e a política do sistema, você deve ter a permissão gravar ou controle total nesse objeto definido no descritor de segurança. Você também pode executar operações de backup por meio do direito de usuário **gerenciar auditoria e log de segurança** (SeSecurityPrivilege). No entanto, esse direito permite o acesso adicional que não é necessário para executar a operação de lista.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para fazer backup de configurações de política de auditoria por usuário para todos os usuários, configurações de política de auditoria do sistema e todas as opções de auditoria em um arquivo de texto formatado para CSV chamado Auditpolicy. csv, digite:

```
auditpol /backup /file:C:\auditpolicy.csv
```

> [!NOTE]
> Se nenhuma unidade for especificada, o diretório atual será usado.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [restauração de Auditpol](auditpol-restore.md)
