---
title: pubprn
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0bc7f7e3-84e1-4359-b477-7b1a1a0bd639
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 17ca9e98ef9e3423521b03c5c21be4b3f1538b62
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722784"
---
# <a name="pubprn"></a>pubprn

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Publica uma impressora nos serviços de domínio Active Directory.

## <a name="syntax"></a>Sintaxe
```
cscript pubprn {<ServerName> | <UNCprinterpath>} 
LDAP://CN=<Container>,DC=<Container>
```

### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|\<ServerName>|Especifica o nome do servidor Windows que hospeda a impressora que você deseja publicar. Se você não especificar um computador, o computador local será usado.|
|\<> UNCprinterpath|O caminho UNC (Convenção de nomenclatura universal) para a impressora compartilhada que você deseja publicar.|
|LDAP://CN =<Container>, DC =<Container>|Especifica o caminho para o contêiner nos serviços de domínio Active Directory em que você deseja publicar a impressora.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários
-   O comando **Pubprn** é um script Visual Basic localizado no diretório%windir%\system32\ printing_Admin_Scripts\\ <language> . Para usar esse comando, em um prompt de comando, digite **cscript** seguido pelo caminho completo para o arquivo Pubprn ou altere os diretórios para a pasta apropriada. Por exemplo: 
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\pubprn
    ```
-   Se as informações fornecidas contiverem espaços, use aspas em volta do texto (por exemplo, `computer Name`).

## <a name="examples"></a>Exemplos
Para publicar todas as impressoras no \\computador \Server1 para o contêiner MyContainer no domínio mydomain.Company.com, digite:
```
cscript Ppubprn Server1 LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com
```
Para publicar a impressora Laserprinter1 no servidor \\\Server1 para o contêiner MyContainer no domínio mydomain.Company.com, digite:
```
cscript Ppubprn \\Server1\Laserprinter1 LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com
```

## <a name="additional-references"></a>Referências adicionais
- [Referência de comando de impressão de chave](command-line-syntax-key.md)
[print Command Reference](print-command-reference.md) de sintaxe de linha de comando
