---
title: scwcmd transform
description: Artigo de referência para o comando scwcmd transform, que transforma um arquivo de política de segurança gerado usando o ACS (Assistente de configuração de segurança) em um novo objeto de Política de Grupo (GPO) no Active Directory Domain Services.
ms.topic: reference
ms.assetid: 640dd892-0bb9-416d-8318-60a26605bcf4
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: bef3d9800d19ac0e3e574b4ef2e1195dcdc3baed
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389249"
---
# <a name="scwcmd-transform"></a>scwcmd transform

> Aplica-se a: Windows Server 2012 R2 e Windows Server 2012

Transforma um arquivo de política de segurança gerado usando o ACS (Assistente de configuração de segurança) em um novo objeto de Política de Grupo (GPO) no Active Directory Domain Services. A operação de transformação não altera as configurações no servidor onde ele é executado. Após a conclusão da operação de transformação, um administrador deve vincular o GPO às UOs desejadas para implantar a política nos servidores.

> [!IMPORTANT]
> As credenciais de administrador de domínio são necessárias para concluir a operação de transformação.
>
> As configurações da política de segurança do Serviços de Informações da Internet (IIS) não podem ser implantadas usando Política de Grupo.
>
> As políticas de firewall que listam aplicativos aprovados não devem ser implantadas em servidores, a menos que o serviço de firewall do Windows seja iniciado automaticamente quando o servidor foi iniciado

## <a name="syntax"></a>Sintaxe

```
scwcmd transform /p:<policyfile.xml> /g:<GPOdisplayname>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /p`<policyfile.xml>` | Especifica o caminho e o nome de arquivo do arquivo de política. XML que deve ser aplicado. Esse parâmetro deve ser especificado. |
| /g`<GPOdisplayname>` | Especifica o nome de exibição do GPO. Esse parâmetro deve ser especificado. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para criar um GPO chamado *FileServerSecurity* a partir de um arquivo chamado *FileServerPolicy.xml*, digite:

```
scwcmd transform /p:FileServerPolicy.xml /g:FileServerSecurity
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando scwcmd analyze](scwcmd-analyze.md)

- [comando de configuração de scwcmd](scwcmd-configure.md)

- [comando de registro scwcmd](scwcmd-register.md)

- [comando scwcmd rollback](scwcmd-rollback.md)

- [comando scwcmd View](scwcmd-view.md)