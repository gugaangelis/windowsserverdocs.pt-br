---
title: tsprof
description: Artigo de referência do comando tsprof, que copia as informações de configuração Serviços de Área de Trabalho Remota usuário de um usuário para outro.
ms.topic: reference
ms.assetid: 27047868-b706-4208-b7e0-1437a2325dd3
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e18f736a31af3cc649d4921197710f713e7df6af
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156343"
---
# <a name="tsprof"></a>tsprof

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia as informações de configuração de usuário Serviços de Área de Trabalho Remota de um usuário para outro. As informações de configuração de usuário Serviços de Área de Trabalho Remota são exibidas nas extensões de Serviços de Área de Trabalho Remota para usuários e grupos locais e usuários e computadores do Active Directory.

> [!NOTE]
> Você também pode usar o [comando tsprof](tsprof.md) para definir o caminho do perfil para um usuário.
>
> Para descobrir as novidades da versão mais recente, consulte Novidades do [serviços de área de trabalho remota no Windows Server](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11)).

## <a name="syntax"></a>Sintaxe

```
tsprof /update {/domain:<Domainname> | /local} /profile:<path> <username>
tsprof /copy {/domain:<Domainname> | /local} [/profile:<path>] <src_user> <dest_user>
tsprof /q {/domain:<Domainname> | /local} <username>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /update | Atualiza informações de caminho de perfil para `<username>` no domínio `<domainname>` para `<profilepath>` . |
| /Domain`<Domainname>` | Especifica o nome do domínio no qual a operação é aplicada. |
| /local | Aplica a operação somente a contas de usuário locais. |
| /Profile`<path>` | Especifica o caminho do perfil, conforme exibido nas extensões de Serviços de Área de Trabalho Remota em usuários e grupos locais e em usuários e computadores do Active Directory. |
| `<username>` | Especifica o nome do usuário para o qual você deseja atualizar ou consultar o caminho do perfil do servidor. |
| /Copy | Copia informações de configuração do usuário do `<src_user>` para `<dest_user>` o e atualiza as informações de caminho do perfil para o `<dest_user>` `<profilepath>` . Ambos `<src_user>` e `<dest_user>` devem ser locais ou devem estar no domínio `<domainname>` . |
| `<src_user>` | Especifica o nome do usuário do qual você deseja copiar as informações de configuração do usuário. Também conhecido como o usuário de origem. |
| `<dest_user>` | Especifica o nome do usuário para o qual você deseja copiar as informações de configuração do usuário. Também conhecido como o usuário de destino. |
| /q | Exibe o caminho do perfil atual do usuário para o qual você deseja consultar o caminho do perfil do servidor. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para copiar as informações de configuração do usuário de *LocalUser1* para *LocalUser2*, digite:

```
tsprof /copy /local LocalUser1 LocalUser2
```

Para definir o caminho do perfil de Serviços de Área de Trabalho Remota para *LocalUser1* como um diretório chamado *c:\Profiles*, digite:

```
tsprof /update /local /profile:c:\profiles LocalUser1
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Referência aos comandos dos Serviços de Área de Trabalho Remota (Serviços de Terminal)](remote-desktop-services-terminal-services-command-reference.md)
