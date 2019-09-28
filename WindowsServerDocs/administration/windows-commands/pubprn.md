---
title: pubprn
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 844a13c1a650ebedcc0d5b4fbf65b9de671b2180
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372014"
---
# <a name="pubprn"></a>pubprn

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Publica uma impressora nos serviços de domínio Active Directory.

## <a name="syntax"></a>Sintaxe
```
cscript pubprn {<ServerName> | <UNCprinterpath>} 
"LDAP://CN=<Container>,DC=<Container>"
```

## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|\<ServerName >|Especifica o nome do servidor Windows que hospeda a impressora que você deseja publicar. Se você não especificar um computador, o computador local será usado.|
|\<UNCprinterpath >|O caminho UNC (Convenção de nomenclatura universal) para a impressora compartilhada que você deseja publicar.|
|"LDAP://CN = <Container>, DC = <Container>"|Especifica o caminho para o contêiner nos serviços de domínio Active Directory em que você deseja publicar a impressora.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários
-   O comando **Pubprn** é um script Visual Basic localizado no diretório%WINdir%\System32\printing_Admin_Scripts @ no__t-1 @ no__t-2. Para usar esse comando, em um prompt de comando, digite **cscript** seguido pelo caminho completo para o arquivo Pubprn ou altere os diretórios para a pasta apropriada. Por exemplo:
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\pubprn
    ```
-   Se as informações fornecidas contiverem espaços, use aspas ao contrário do texto (por exemplo, `"computer Name"`).

## <a name="BKMK_examples"></a>Disso
Para publicar todas as impressoras no computador \\ \ Server1 para o contêiner MyContainer no domínio MyDomain.company.Com, digite:
```
cscript Ppubprn Server1 "LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com"
```
Para publicar a impressora Laserprinter1 no servidor \\ \ Server1 para o contêiner MyContainer no domínio MyDomain.company.Com, digite:
```
cscript Ppubprn \\Server1\Laserprinter1 "LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com"
```

#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[referência de comando de impressão](print-command-reference.md)
