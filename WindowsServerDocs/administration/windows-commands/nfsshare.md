---
title: nfsshare
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 50df88a3fddbceb1595f328bdd4e3c6f526e3a2c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881687"
---
# <a name="nfsshare"></a>nfsshare



Você pode usar **nfsshare** ao controle do sistema de arquivos de rede (NFS) compartilha.

## <a name="syntax"></a>Sintaxe

```
nfsshare <ShareName>=<Drive:Path> [-o <Option=value>...]
nfsshare {<ShareName> | <Drive>:<Path> | * } /delete
```

## <a name="description"></a>Descrição

Sem argumentos, o **nfsshare** utilitário de linha de comando lista todos os compartilhamentos de sistema de arquivos de rede (NFS) exportados pelo servidor para NFS. Com o *ShareName* como o único argumento, **nfsshare** lista as propriedades do compartilhamento NFS identificado pela *ShareName*. Quando *ShareName* e *unidade ***:*** caminho* forem fornecidas, **nfsshare** exporta a pasta identificada pelo *daunidade ***:*** Caminho* como *ShareName*. Quando o **/excluir** opção for usada, a pasta especificada não fica disponível para clientes NFS.

## <a name="options"></a>Opções

O **nfsshare** comando aceita as seguintes opções e argumentos:

|Termo|Definição|
|----|----------|
|-o anon = {Sim | no}|Especifica se (não mapeados) de usuários anônimos podem acessar o diretório compartilhado. O padrão é **nenhum**.|
|-o rw[=\<Host>[:<Host>]...]|Fornece acesso de leitura e gravação para o diretório compartilhado pelo cliente ou hosts grupos especificados pelo *Host*. Separe os nomes de host e o grupo com dois-pontos (**:**). Se *Host* não for especificado, todos os hosts e grupos de cliente (exceto aquelas especificadas com o **ro** opção) têm acesso de leitura / gravação. Se nem o **ro** nem a **rw** opção for definida, todos os clientes têm acesso de leitura e gravação para o diretório compartilhado.|
|-o ro[=\<Host>[:<Host>]...]|Fornece acesso somente leitura para o diretório compartilhado pelo cliente ou hosts grupos especificados pelo *Host*. Separe os nomes de host e o grupo com dois-pontos (**:**). Se *Host* não for especificado, todos os clientes (exceto aquelas especificadas com o **rw** opção) têm acesso somente leitura. Se o **ro** opção for definida para um ou mais clientes, mas o **rw** opção não for definida, somente os clientes especificado com o **ro** opção ter acesso à pasta compartilhada.|
|-o encoding={big5|euc-jp|euc-kr|euc-tw|gb2312-80|ksc5601|shift-jis}|Especifica a codificação padrão usado para nomes de arquivo e diretório e, se usado, deve ser definida para um dos seguintes:</br>-   **Big5** (chinês)</br>-   **euc-jp** (Japanese)</br>-   **Coreia EUC** (coreano)</br>-   **chinês-Taiwan-EUC** (chinês)</br>-   **gb2312-80** (chinês simplificado)</br>-   **KSC5601** (coreano)</br>-   **SHIFT-jis** (japonês)</br>Se esta opção não for definido, o esquema de codificação padrão é ANSI ou, em sistemas configurados para localidades do inglês, a codificação de esquema para a localidade padrão. Estes são os esquemas de codificação padrão para as localidades indicadas:</br>-Japonês: SHIFT-JIS</br>-Coreano: KS_C_5601-1987</br>-Chinês simplificado: GB2312-80</br>Chinês tradicional: BIG5|
|-o anongid=\<gid>|Especifica que os usuários (não mapeados) anônimos terão acesso o diretório de compartilhamento usando *gid* como seu identificador de grupo (GID). O padrão é -2. O GID anônimo será usado ao relatar o proprietário de um arquivo pertencente a um usuário não mapeado, mesmo se o acesso anônimo é desabilitado.|
|-o  anonuid=\<uid>|Especifica que os usuários (não mapeados) anônimos terão acesso o diretório de compartilhamento usando *uid* como seu identificador de usuário (UID). O padrão é -2. O UID anônimo será usado ao relatar o proprietário de um arquivo pertencente a um usuário não mapeado, mesmo se o acesso anônimo é desabilitado.|
|-o root[=\<Host>[:<Host>]...]|Fornece acesso à raiz para o diretório compartilhado pelos hosts ou cliente grupos especificados pelo *Host*. Separe os nomes de host e o grupo com dois-pontos (**:**). Se *Host* não for especificado, todos os clientes tiverem acesso à raiz. Se o **raiz** opção não estiver definida, nenhum cliente têm acesso à raiz para o diretório compartilhado.|
|/delete|Se *ShareName* ou *unidade ***:*** caminho* for especificado, exclui o compartilhamento especificado. Se * estiver especificado, exclui todos os compartilhamentos NFS.|

> [!NOTE]
> Para exibir a sintaxe completa desse comando, no prompt de comando, digite:</br>> **nfsshare /?**

##

[Referência de comandos do sistema de arquivos de serviços de rede](services-for-network-file-system-command-reference.md) Consulte também