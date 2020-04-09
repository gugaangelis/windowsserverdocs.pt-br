---
title: auditpol
description: O tópico comandos do Windows para **Auditpol**, que exibe informações sobre e executa funções para manipular políticas de auditoria.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a02cfb9d-732f-4e77-aeba-f18265daa3af
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 00365b0e46b8bff761cf991dbdbd09d8f5e9c687
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851129"
---
# <a name="auditpol"></a>auditpol

Exibe informações sobre e executa funções para manipular políticas de auditoria.

Para obter exemplos de como esse comando pode ser usado, consulte a seção exemplos em cada tópico.

## <a name="syntax"></a>Sintaxe

```
auditpol command [<sub-command><options>]
```

#### <a name="parameters"></a>Parâmetros

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

## <a name="remarks"></a>Comentários

A ferramenta de linha de comando de diretiva de auditoria pode ser usada para:

- Definir e consultar uma política de auditoria do sistema.

- Definir e consultar uma política de auditoria por usuário.

- Definir e consultar opções de auditoria.

- Defina e consulte o descritor de segurança usado para delegar acesso a uma política de auditoria.

- Relatar ou fazer backup de uma política de auditoria em um arquivo de texto de valores separados por vírgulas (CSV).

- Carregar uma política de auditoria de um arquivo de texto CSV.

- Configure as SACLs de recursos globais.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)