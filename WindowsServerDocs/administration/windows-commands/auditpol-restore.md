---
title: restauração de Auditpol
description: O tópico de comandos do Windows para o **Auditpol Restore** -restaura as configurações da política de auditoria do sistema, as configurações de política de auditoria por usuário para todos os usuários e todas as opções de auditoria de um arquivo que é sintaticamente consistente com o formato de arquivo CSV (valores separados por vírgula) usado pela opção/backup.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ad73e520-484f-4cf1-a7f9-ae7488e9edf6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b91f3745354c695c4ab0c71b429718bff05d8098
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382414"
---
# <a name="auditpol-restore"></a>restauração de Auditpol

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Restaura as configurações da política de auditoria do sistema, as configurações de política de auditoria por usuário para todos os usuários e todas as opções de auditoria de um arquivo que é sintaticamente consistente com o formato de arquivo CSV (valores separados por vírgula) usado pela opção/backup.

## <a name="syntax"></a>Sintaxe
```
auditpol /restore /file:<filename>
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/File|Especifica o arquivo do qual a política de auditoria deve ser restaurada. O arquivo deve ter sido criado usando a opção/backup ou deve ser sintaticamente consistente com o formato de arquivo CSV usado pela opção/backup.|
|/?|Exibe a ajuda no prompt de comando.|
## <a name="remarks"></a>Comentários
para operações de restauração para a política por usuário e política do sistema, você deve ter a permissão gravar ou controle total nesse objeto definido no descritor de segurança. Você também pode executar a operação de restauração por meio do direito de usuário **gerenciar auditoria e log de segurança** (SeSecurityPrivilege). SeSecurityPrivilege é útil ao restaurar o descritor de segurança no caso de um erro inadvertido ou um ataque mal-intencionado.
## <a name="BKMK_examples"></a>Disso
Para restaurar as configurações de política de auditoria do sistema, as configurações de política de auditoria por usuário para todos os usuários e todas as opções de auditoria de um arquivo chamado Auditpolicy. csv que foi criado usando o comando/backup, digite:
```
auditpol /restore /file:c:\auditpolicy.csv
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[backup de Auditpol](auditpol-backup.md)
