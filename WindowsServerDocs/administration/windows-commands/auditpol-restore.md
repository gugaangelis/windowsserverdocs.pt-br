---
title: auditpol restore
description: Artigo de referência do comando Auditpol Restore, que restaura as configurações da política de auditoria do sistema, as configurações de política de auditoria por usuário para todos os usuários e todas as opções de auditoria de um arquivo que é sintaticamente consistente com o formato de arquivo CSV (valores separados por vírgula) usado pela opção/backup.
ms.topic: reference
ms.assetid: ad73e520-484f-4cf1-a7f9-ae7488e9edf6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d6dc8dde77189cbd1134779bf89f253402f181dc
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633115"
---
# <a name="auditpol-restore"></a>auditpol restore

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Restaura as configurações da política de auditoria do sistema, as configurações de política de auditoria por usuário para todos os usuários e todas as opções de auditoria de um arquivo que é sintaticamente consistente com o formato de arquivo CSV (valores separados por vírgula) usado pela opção/backup.

Para executar operações de *restauração* nas políticas *por usuário* e *sistema* , você deve ter a permissão **gravar** ou **controle total** para esse objeto definido no descritor de segurança. Você também pode executar operações de *restauração* se tiver o direito de usuário **gerenciar auditoria e log de segurança** (SeSecurityPrivilege), que é útil ao restaurar o descritor de segurança em caso de erro ou ataque mal-intencionado.

## <a name="syntax"></a>Sintaxe

```
auditpol /restore /file:<filename>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| ------- | -------- |
| /File | Especifica o arquivo do qual a política de auditoria deve ser restaurada. O arquivo deve ter sido criado usando a opção/backup ou deve ser sintaticamente consistente com o formato de arquivo CSV usado pela opção/backup. |
| /? |Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para restaurar as configurações de política de auditoria do sistema, as configurações de política de auditoria por usuário para todos os usuários e todas as opções de auditoria de um arquivo chamado auditpolicy.csv que foi criado usando o comando/backup, digite:

```
auditpol /restore /file:c:\auditpolicy.csv
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [auditpol backup](auditpol-backup.md)

- [comandos Auditpol](auditpol.md)
