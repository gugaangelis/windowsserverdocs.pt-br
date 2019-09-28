---
title: nfsshare
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 437a2615-335a-442f-9713-d50d5f3983a3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a952e247ee40f832045d39d0e2164bb2e6613c54
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373213"
---
# <a name="nfsshare"></a>nfsshare



Você pode usar o **nfsshare** para controlar compartilhamentos de NFS (sistema de arquivos de rede).

## <a name="syntax"></a>Sintaxe

```
nfsshare <ShareName>=<Drive:Path> [-o <Option=value>...]
nfsshare {<ShareName> | <Drive>:<Path> | * } /delete
```

## <a name="description"></a>Descrição

Sem argumentos, o utilitário de linha de comando **nfsshare** lista todos os compartilhamentos de NFS (sistema de arquivos de rede) exportados pelo servidor para NFS. Com *ShareName* como o único argumento, **nfsshare** lista as propriedades do compartilhamento NFS identificado por *ShareName*. Quando *ShareName* e <em>drive</em> **:** <em>caminho</em> são fornecidos, **nfsshare** exporta a pasta identificada por <em>unidade</em> **:** <em>caminho</em> como *ShareName*. Quando a opção **/delete** é usada, a pasta especificada não é mais disponibilizada para clientes NFS.

## <a name="options"></a>Opções

O comando **nfsshare** aceita as seguintes opções e argumentos:


|             Termo              |                                                                                                                                                                                                                      Definição                                                                                                                                                                                                                       |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         -o anon = {Sim          |                                                                                                                                                                                                                          foi                                                                                                                                                                                                                          |
|  -o RW [= \<Host > [: <Host>]...]  |                       Fornece acesso de leitura/gravação ao diretório compartilhado pelos hosts ou grupos de clientes especificados pelo *host*. Separe os nomes de host e de grupo com dois-pontos ( **:** ). Se o *host* não for especificado, todos os hosts e grupos de clientes (exceto aqueles especificados com a opção **ro** ) terão acesso de leitura/gravação. Se nem a opção **ro** nem a **RW** estiver definida, todos os clientes terão acesso de leitura/gravação ao diretório compartilhado.                       |
|  -o ro [= \<Host > [: <Host>]...]  | Fornece acesso somente leitura ao diretório compartilhado pelos hosts ou grupos de clientes especificados pelo *host*. Separe os nomes de host e de grupo com dois-pontos ( **:** ). Se o *host* não for especificado, todos os clientes (exceto aqueles especificados com a opção **RW** ) terão acesso somente leitura. Se a opção **ro** for definida para um ou mais clientes, mas a opção **RW** não estiver definida, somente os clientes especificados com a opção **ro** terão acesso ao diretório compartilhado. |
|       -o Encoding = {Big5       |                                                                                                                                                                                                                        euc-jp                                                                                                                                                                                                                         |
|       -o anongid = \<gid >       |                                                                                     Especifica que usuários anônimos (não mapeados) acessarão o diretório de compartilhamento usando o *GID* como seu GID (identificador de grupo). O padrão é-2. O GID anônimo será usado ao relatar o proprietário de um arquivo de propriedade de um usuário não mapeado, mesmo que o acesso anônimo esteja desabilitado.                                                                                      |
|      -o anonuid = \<uid >       |                                                                                      Especifica que usuários anônimos (não mapeados) acessarão o diretório de compartilhamento usando *UID* como seu UID (identificador de usuário). O padrão é-2. A UID anônima será usada ao relatar o proprietário de um arquivo de propriedade de um usuário não mapeado, mesmo que o acesso anônimo esteja desabilitado.                                                                                      |
| -o root [= \<Host > [: <Host>]...] |                                                                         Fornece acesso de raiz ao diretório compartilhado pelos hosts ou grupos de clientes especificados pelo *host*. Separe os nomes de host e de grupo com dois-pontos ( **:** ). Se o *host* não for especificado, todos os clientes terão acesso à raiz. Se a opção **raiz** não for definida, nenhum cliente terá acesso à raiz para o diretório compartilhado.                                                                         |
|            /delete            |                                                                                                                                                       Se *ShareName* ou <em>drive</em> **:** <em>caminho</em> for especificado, o excluirá o compartilhamento especificado. Se \* for especificado, excluirá todos os compartilhamentos NFS.                                                                                                                                                       |

> [!NOTE]
> Para exibir a sintaxe completa desse comando, no prompt de comando, digite:</br>> **nfsshare/?**

# #

[Serviços para referência de comando do sistema de arquivos de rede](services-for-network-file-system-command-reference.md) Consulte também