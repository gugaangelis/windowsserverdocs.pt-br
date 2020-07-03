---
title: nfsshare
description: Artigo de referência para o comando nfsshare, que controla compartilhamentos de NFS (sistema de arquivos de rede).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 437a2615-335a-442f-9713-d50d5f3983a3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4901e0c9ee0701261dc6abb8cfd69cc02d4dd02e
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85932042"
---
# <a name="nfsshare"></a>nfsshare

Controla compartilhamentos NFS (Network File System). Usado sem parâmetros, esse comando exibe todos os compartilhamentos de NFS (sistema de arquivos de rede) exportados pelo servidor para NFS.

## <a name="syntax"></a>Sintaxe

```
nfsshare <sharename>=<drive:path> [-o <option=value>...]
nfsshare {<sharename> | <drive>:<path> | * } /delete
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| -o anon =`{yes|no}` | Especifica se usuários anônimos (não mapeados) podem acessar o diretório de compartilhamento. |
| -o RW =`[<host>[:<host>]...]` | Fornece acesso de leitura/gravação ao diretório compartilhado pelos hosts ou grupos de clientes especificados pelo *host*. Você deve separar os nomes de host e de grupo com dois-pontos (**:**). Se o *host* não for especificado, todos os hosts e grupos de clientes (exceto aqueles especificados com a opção **ro** ) obterão acesso de leitura/gravação. Se nem a opção **ro** nem a **RW** estiver definida, todos os clientes terão acesso de leitura/gravação ao diretório compartilhado. |
| -o ro =`[<host>[:<host>]...]` | Fornece acesso somente leitura ao diretório compartilhado pelos hosts ou grupos de clientes especificados pelo *host*. Você deve separar os nomes de host e de grupo com dois-pontos (**:**). Se o *host* não for especificado, todos os clientes (exceto aqueles especificados com a opção **RW** ) obterão acesso somente leitura. Se a opção **ro** for definida para um ou mais clientes, mas a opção **RW** não estiver definida, somente os clientes especificados com a opção **ro** terão acesso ao diretório compartilhado. |
| -o codificação =`{euc-jp|euc-tw|euc-kr|shift-jis|Big5|Ksc5601|Gb2312-80|Ansi)` | Especifica a codificação de linguagem a ser configurada em um compartilhamento NFS. Você pode usar apenas um idioma no compartilhamento. Esse valor pode incluir qualquer um dos seguintes valores:<ul><li>**EUC-JP:** Japonês</li><li>**EUC-TW:** Chinês</li><li>**EUC-Kr:** Coreano</li><li>**Shift-JIS:** Japonês</li><li>**Big5:** Chinês</li><li>**Ksc5601:** Coreano</li><li>**Gb2312-80:** Chinês simplificado</li><li>**ANSI:** Codificado em ANSI</li></ul> |
| -o anongid =`<gid>` | Especifica que usuários anônimos (não mapeados) acessem o diretório de compartilhamento usando o *GID* como seu GID (identificador de grupo). O padrão é **-2**. O GID anônimo é usado ao relatar o proprietário de um arquivo de propriedade de um usuário não mapeado, mesmo que o acesso anônimo esteja desabilitado. |
| -o anonuid =`<uid>` | Especifica que usuários anônimos (não mapeados) acessem o diretório de compartilhamento usando *UID* como seu UID (identificador de usuário). O padrão é **-2**. A UID anônima é usada ao relatar o proprietário de um arquivo de propriedade de um usuário não mapeado, mesmo que o acesso anônimo esteja desabilitado. |
| -o raiz =`[<host>[:<host>]...]` | Fornece acesso de raiz ao diretório compartilhado pelos hosts ou grupos de clientes especificados pelo *host*. Você deve separar os nomes de host e de grupo com dois-pontos (**:**). Se o *host* não for especificado, todos os clientes obterão acesso à raiz. Se a opção **raiz** não estiver definida, nenhum cliente terá acesso à raiz para o diretório compartilhado. |
| /delete | Se *ShareName* ou `<drive>:<path>` for especificado, esse parâmetro excluirá o compartilhamento especificado. Se um caractere curinga (*) for especificado, esse parâmetro excluirá todos os compartilhamentos NFS. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Se *ShareName* como o único parâmetro, esse comando listará as propriedades do compartilhamento NFS identificado por *ShareName*.

- Se *ShareName* e `<drive>:<path>` forem usados, esse comando exporta a pasta identificada por `<drive>:<path>` como *ShareName*. Se você usar a opção **/delete** , a pasta especificada deixará de estar disponível para clientes NFS.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Referência aos comandos dos Serviços para Network File System](services-for-network-file-system-command-reference.md)

- [Referência de cmdlets do NFS](https://docs.microsoft.com/powershell/module/nfs)
