---
title: Scwcmd transform
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 640dd892-0bb9-416d-8318-60a26605bcf4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 36ee3a99828c7fdd9d4fc0ca14cbc0e203b01ea0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384313"
---
# <a name="scwcmd-transform"></a>Scwcmd: transform

> Aplica-se a: Windows Server 2012 R2, Windows Server 2012

Transforma um arquivo de política de segurança gerado usando o ACS (Assistente de configuração de segurança) em um novo objeto de Política de Grupo (GPO) no Active Directory Domain Services. A operação de transformação não altera as configurações no servidor onde ele é executado. Após a conclusão da operação de transformação, um administrador deve vincular o GPO às UOs desejadas para implantar a política nos servidores.

As credenciais de administrador de domínio são necessárias para concluir a operação de transformação.

> [!IMPORTANT]
> As configurações de política de segurança do Serviços de Informações da Internet (IIS) não podem ser implantadas usando Política de Grupo.</br>> Políticas de firewall que listam aplicativos aprovados não devem ser implantados em servidores, a menos que o serviço de firewall do Windows seja iniciado automaticamente quando o servidor foi iniciado pela última vez.

Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
scwcmd transform /p:<Policyfile.xml> /g:<GPODisplayName>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/p: @no__t -0Policyfile. XML >|Especifica o caminho e o nome de arquivo do arquivo de política. XML que deve ser aplicado. Esse parâmetro deve ser especificado.|
|/g: \<GPODisplayName >|Especifica o nome de exibição do GPO. Esse parâmetro deve ser especificado.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

Scwcmd. exe só está disponível em computadores que executam o Windows Server 2008 R2, o Windows Server 2008 ou o Windows Server 2003.

## <a name="BKMK_Examples"></a>Disso

Para criar um GPO chamado FileServerSecurity a partir de um arquivo chamado FileServerPolicy. xml, digite:
```
scwcmd transform /p:FileServerPolicy.xml /g:FileServerSecurity
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)