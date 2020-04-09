---
title: tsprof
description: O tópico de comandos do Windows para tsprof, que copia as informações de configuração Serviços de Área de Trabalho Remota usuário de um usuário para outro.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 27047868-b706-4208-b7e0-1437a2325dd3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3b1037b6daff467a71517917d423e4cbe87a97f2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832469"
---
# <a name="tsprof"></a>tsprof

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia as informações de configuração de usuário Serviços de Área de Trabalho Remota de um usuário para outro.
As informações de configuração de usuário Serviços de Área de Trabalho Remota são exibidas nas extensões de Serviços de Área de Trabalho Remota para usuários e grupos locais e usuários e computadores do Active Directory.

**tsprof** também pode definir o caminho do perfil para um usuário.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

> [!NOTE]
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir as novidades da versão mais recente, consulte [novidades do serviços de área de trabalho remota no Windows server 2012](https://technet.microsoft.com/library/hh831527) na biblioteca do TechNet do Windows Server.

## <a name="syntax"></a>Sintaxe
```
tsprof /update {/domain:<DomainName> | /local} /profile:<path> <UserName>
tsprof /copy {/domain:<DomainName> | /local} [/profile:<path>] <Src_usr> <Dest_usr>
tsprof /q {/domain:<DomainName> | /local} <UserName>
```

### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|{1&gt;{2&gt;/update&lt;2}&lt;1}|Atualiza informações de caminho de perfil para <*nome de usuário*> no*domínio < o*> do < do*ProfilePath*>.|
|/domain:\<nome_do_domínio >|Especifica o nome do domínio no qual a operação é aplicada.|
|/local|Aplica a operação somente a contas de usuário locais.|
|/Profile: caminho de\<>|Especifica o caminho do perfil, conforme exibido nas extensões de Serviços de Área de Trabalho Remota em usuários e grupos locais e em usuários e computadores do Active Directory.|
|\<nome de usuário >|Especifica o nome do usuário para o qual você deseja atualizar ou consultar o caminho do perfil do servidor.|
|/Copy|Copia as informações de configuração do usuário de \<*SourceUser*> para \<*DestinationUser*> e atualiza as informações de caminho do perfil para \<*DestinationUser*> para \<*ProfilePath*>. Tanto \<*SourceUser*> quanto \<*DestinationUser*> devem ser locais ou devem estar no domínio \<*DomainName*>.|
|\<Src_usr >|Especifica o nome do usuário do qual você deseja copiar as informações de configuração do usuário.|
|\<Dest_usr >|Especifica o nome do usuário para o qual você deseja copiar as informações de configuração do usuário.|
|/q|Exibe o caminho do perfil atual do usuário para o qual você deseja consultar o caminho do perfil do servidor.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários
-   O comando **tsprof** só está disponível quando você instalou o serviço de função Terminal Server em um computador que executa o windows Server 2008 ou o serviço de função host da Sessão RD em um computador que executa o windows Server 2008 R2.

## <a name="examples"></a><a name=BKMK_examples></a>Disso
-   Para copiar as informações de configuração do usuário de LocalUser1 para LocalUser2, digite:
    ```
    tsprof /copy /local LocalUser1 LocalUser2
    ```
-   Para definir o caminho do perfil de Serviços de Área de Trabalho Remota para LocalUser1 como um diretório chamado c:\Profiles, digite:
    ```
    tsprof /update /local /profile:c:\profiles LocalUser1
    ```

## <a name="additional-references"></a>Referências adicionais
- [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[referência de comando serviços de área de trabalho remota (serviços de terminal)](remote-desktop-services-terminal-services-command-reference.md)
