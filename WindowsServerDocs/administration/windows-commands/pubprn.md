---
title: pubprn
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0bc7f7e3-84e1-4359-b477-7b1a1a0bd639
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 499ff2ade7ffc6c608791ba3da0ede0c3282c13d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831697"
---
# <a name="pubprn"></a>pubprn

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Publica uma impressora para serviços de domínio do active directory.

## <a name="syntax"></a>Sintaxe
```
cscript pubprn {<ServerName> | <UNCprinterpath>} 
"LDAP://CN=<Container>,DC=<Container>"
```

## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|\<ServerName>|Especifica o nome do servidor Windows que hospeda a impressora que você deseja publicar. Se você não especificar um computador, o computador local será usado.|
|\<UNCprinterpath>|O caminho de convenção de nomenclatura Universal (UNC) para a impressora compartilhada que você deseja publicar.|
|"LDAP://CN=<Container>,DC=<Container>"|Especifica o caminho para o contêiner nos serviços de domínio do Active Directory onde você deseja publicar a impressora.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários
-   O **pubprn** comando é um script do Visual Basic localizado no %WINdir%\System32\printing_Admin_Scripts\\ <language> directory. Para usar este comando no prompt de comando, digite **cscript** seguido pelo caminho completo do arquivo pubprn ou altere os diretórios para a pasta apropriada. Por exemplo: 
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\pubprn
    ```
-   Se as informações que você fornece contiverem espaços, use aspas ao redor do texto (por exemplo, `"computer Name"`).

## <a name="BKMK_examples"></a>Exemplos
Para publicar todas as impressoras no \\\Server1 computador para o contêiner MyContainer no domínio MyDomain.company.Com, digite:
```
cscript Ppubprn Server1 "LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com"
```
Para publicar a impressora Laserprinter1 no \\\Server1 server para o contêiner MyContainer no domínio MyDomain.company.Com, tipo:
```
cscript Ppubprn \\Server1\Laserprinter1 "LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com"
```

#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[referência do comando Imprimir](print-command-reference.md)
