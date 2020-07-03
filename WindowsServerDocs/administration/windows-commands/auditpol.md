---
title: auditpol
description: Artigo de referência para o comando Auditpol, que exibe informações sobre e executa funções para manipular políticas de auditoria.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a02cfb9d-732f-4e77-aeba-f18265daa3af
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f3f503b957175ba2a3997202d83c171cf8683032
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923662"
---
# <a name="auditpol"></a>auditpol

Exibe informações sobre e executa funções para manipular políticas de auditoria, incluindo:

- Configurando e consultando uma política de auditoria do sistema.

- Configurando e consultando uma política de auditoria por usuário.

- Configurando e consultando opções de auditoria.

- Configurando e consultando o descritor de segurança usado para delegar acesso a uma política de auditoria.

- Relatando ou fazendo backup de uma política de auditoria para um arquivo de texto de valores separados por vírgulas (CSV).

- Carregando uma política de auditoria de um arquivo de texto CSV.

- Configurando SACLs de recursos globais.

## <a name="syntax"></a>Sintaxe

```
auditpol command [<sub-command><options>]
```

### <a name="parameters"></a>Parâmetros

| Subcomando | Descrição |
| ----------- | ----------- |
| /Get | Exibe a política de auditoria atual. Para obter mais informações, consulte [Auditpol Get](auditpol-get.md) para sintaxe e opções. |
| /Set | Define a política de auditoria. Para obter mais informações, consulte [Auditpol set](auditpol-set.md) para sintaxe e opções. |
| /list | Exibe elementos de política selecionáveis. Para obter mais informações, consulte [lista de Auditpol](auditpol-list.md) para sintaxe e opções. |
| /backup | Salva a política de auditoria em um arquivo. Para obter mais informações, consulte [backup de Auditpol](auditpol-backup.md) para sintaxe e opções. |
| /Restore | Restaura a política de auditoria de um arquivo criado anteriormente usando Auditpol/backup. Para obter mais informações, consulte [Auditpol Restore](auditpol-restore.md) para sintaxe e opções. |
| /Clear | Limpa a política de auditoria. Para obter mais informações, consulte [Auditpol Clear](auditpol-clear.md) para sintaxe e opções. |
| /remove | Remove todas as configurações de política de auditoria por usuário e desabilita todas as configurações de política de auditoria do sistema. Para obter mais informações, consulte [Auditpol remove](auditpol-remove.md) para sintaxe e opções. |
| /resourceSACL | Configura as SACLs (listas de controle de acesso) do sistema de recursos globais. **Observação:** Aplica-se somente ao Windows 7 e ao Windows Server 2008 R2. Para obter mais informações, consulte [Auditpol resourceSACL](auditpol-resourcesacl.md). |
| /?| Exibe a ajuda no prompt de comando. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
