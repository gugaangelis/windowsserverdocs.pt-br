---
title: pubprn
description: Artigo de referência para o comando Pubprn, que publica uma impressora no Active Directory Domain Services.
ms.topic: article
ms.assetid: 0bc7f7e3-84e1-4359-b477-7b1a1a0bd639
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 33905fbfe06887ee8b8721ed9c91eed5701ed3f5
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87884605"
---
# <a name="pubprn"></a>pubprn

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Publica uma impressora no Active Directory Domain Services. Esse comando é um script Visual Basic localizado no `%WINdir%\System32\printing_Admin_Scripts\<language>` diretório. Para usar esse comando em um prompt de comando, digite **cscript** seguido pelo caminho completo para o arquivo Pubprn ou altere os diretórios para a pasta apropriada. Por exemplo: `cscript %WINdir%\System32\printing_Admin_Scripts\en-US\pubprn`.

## <a name="syntax"></a>Sintaxe

```
cscript pubprn {<servername> | <UNCprinterpath>} LDAP://CN=<container>,DC=<container>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<servername>` | Especifica o nome do servidor Windows que hospeda a impressora que você deseja publicar. Se você não especificar um computador, o computador local será usado. |
| `<UNCprinterpath>` | O caminho UNC (Convenção de nomenclatura universal) para a impressora compartilhada que você deseja publicar. |
| `LDAP://CN=<Container>,DC=<Container>` | Especifica o caminho para o contêiner em Active Directory Domain Services em que você deseja publicar a impressora. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Se as informações fornecidas contiverem espaços, use aspas ao contrário do texto (por exemplo, "nome do computador").

### <a name="examples"></a>Exemplos

Para publicar todas as impressoras no \\ computador Server1 no contêiner MyContainer no domínio mydomain.Company.com, digite:

```
cscript pubprn Server1 LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com
```

Para publicar a impressora Laserprinter1 no \\ servidor \Server1 para o contêiner MyContainer no domínio mydomain.Company.com, digite:

```
cscript pubprn \\Server1\Laserprinter1 LDAP://CN=MyContainer,DC=MyDomain,DC=company,DC=Com
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Referência aos comandos de impressão](print-command-reference.md)
