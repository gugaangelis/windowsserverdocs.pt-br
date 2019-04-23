---
title: Transformação scwcmd
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 8a6a6e37c2c2a362f3aa0aeadef615ff5065713f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843807"
---
# <a name="scwcmd-transform"></a>Scwcmd: transform

> Aplica-se a: Windows Server 2012 R2, Windows Server 2012

Transforma um arquivo de política de segurança gerado usando o Assistente de configuração de segurança (ACS) em um novo grupo de política de GPO (objeto) nos serviços de domínio do Active Directory. A operação de transformação não altera as configurações no servidor em que ele é executado. Após a operação de transformação, um administrador deve vincular o GPO às UOs para implantar a política aos servidores desejadas.

As credenciais de administrador de domínio são necessárias para concluir a operação de transformação.

> [!IMPORTANT]
> Configurações de política de segurança do Internet Information Services (IIS) não podem ser implantadas usando a diretiva de grupo.</br>> Políticas de firewall que listam aplicativos aprovados não devem ser implantados em servidores, a menos que o serviço de Firewall do Windows é iniciado automaticamente quando o servidor foi iniciado.

Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
scwcmd transform /p:<Policyfile.xml> /g:<GPODisplayName>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/p:\<Policyfile.xml>|Especifica o nome de arquivo e caminho do arquivo de diretiva. XML que deve ser aplicado. Esse parâmetro deve ser especificado.|
|/g:\<GPODisplayName>|Especifica o nome de exibição do GPO. Esse parâmetro deve ser especificado.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

Scwcmd.exe só está disponível em computadores que executam o Windows Server 2008 R2, Windows Server 2008 ou Windows Server 2003.

## <a name="BKMK_Examples"></a>Exemplos

Para criar um GPO chamado FileServerSecurity de um arquivo chamado FileServerPolicy.xml, digite:
```
scwcmd transform /p:FileServerPolicy.xml /g:FileServerSecurity
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)