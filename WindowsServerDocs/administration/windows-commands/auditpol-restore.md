---
title: restauração de Auditpol
description: Tópico de comandos do Windows para **restauração auditpol** -restaura configurações de política de auditoria do sistema, configurações de política de auditoria por usuário para todos os usuários e todas as opções de auditoria de um arquivo que é sintaticamente consistente com o separados por vírgula formato de arquivo CSV (valores) usado pelo /backup opção.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f1961387083a8a61b27f3e44a2380a6060a02f98
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868977"
---
# <a name="auditpol-restore"></a>restauração de Auditpol

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Restaura as configurações de política de auditoria do sistema, configurações de política de auditoria por usuário para todos os usuários e todas as opções de auditoria de um arquivo que é sintaticamente consistente com o formato de arquivo de valores separados por vírgulas (CSV) usado pelo /backup opção.

## <a name="syntax"></a>Sintaxe
```
auditpol /restore /file:<filename>
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/file|Especifica o arquivo do qual a política de auditoria deve ser restaurada. O arquivo deve ter sido criado usando o /backup opção ou deve ser sintaticamente consistente com o formato de arquivo CSV usado pelo /backup opção.|
|/?|Exibe a ajuda no prompt de comando.|
## <a name="remarks"></a>Comentários
para operações de restauração para a política por usuário e a diretiva do sistema, você deve escrever ou conjunto de permissões de controle total nesse objeto no descritor de segurança. Você também pode executar a operação de restauração que possui o **gerenciar o log de auditoria e segurança** direito de usuário (SeSecurityPrivilege). SeSecurityPrivilege é útil ao restaurar o descritor de segurança em caso de erro inadvertido ou ataque mal-intencionado.
## <a name="BKMK_examples"></a>Exemplos
Para restaurar as configurações de política de auditoria do sistema, configurações de política de auditoria por usuário para todos os usuários e todas as opções de auditoria de um arquivo chamado auditpolicy.csv que foi criado usando o /backup comando, digite:
```
auditpol /restore /file:c:\auditpolicy.csv
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[auditpol backup](auditpol-backup.md)
