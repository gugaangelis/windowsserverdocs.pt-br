---
title: Scwcmd transform
description: Artigo de referência para * * * *-
ms.topic: article
ms.assetid: 640dd892-0bb9-416d-8318-60a26605bcf4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5af1f9d1f2ee5386da8b02f4142c156c3711852f
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87883092"
---
# <a name="scwcmd-transform"></a>Scwcmd: transform

> Aplica-se a: Windows Server 2012 R2, Windows Server 2012

Transforma um arquivo de política de segurança gerado usando o ACS (Assistente de configuração de segurança) em um novo objeto de Política de Grupo (GPO) no Active Directory Domain Services. A operação de transformação não altera as configurações no servidor onde ele é executado. Após a conclusão da operação de transformação, um administrador deve vincular o GPO às UOs desejadas para implantar a política nos servidores.

As credenciais de administrador de domínio são necessárias para concluir a operação de transformação.

> [!IMPORTANT]
> As configurações de política de segurança do Serviços de Informações da Internet (IIS) não podem ser implantadas usando Política de Grupo.</br>> políticas de firewall que listam aplicativos aprovados não devem ser implantados em servidores, a menos que o serviço de firewall do Windows seja iniciado automaticamente quando o servidor foi iniciado pela última vez.



## <a name="syntax"></a>Sintaxe

```
scwcmd transform /p:<Policyfile.xml> /g:<GPODisplayName>
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/p\<Policyfile.xml>|Especifica o caminho e o nome de arquivo do arquivo de política. XML que deve ser aplicado. Esse parâmetro deve ser especificado.|
|/g\<GPODisplayName>|Especifica o nome de exibição do GPO. Esse parâmetro deve ser especificado.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

Scwcmd.exe só está disponível em computadores que executam o Windows Server 2008 R2, o Windows Server 2008 ou o Windows Server 2003.

## <a name="examples"></a>Exemplos

Para criar um GPO chamado FileServerSecurity a partir de um arquivo chamado FileServerPolicy.xml, digite:
```
scwcmd transform /p:FileServerPolicy.xml /g:FileServerSecurity
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)