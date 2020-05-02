---
title: 'ksetup: delkpasswd'
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2db0bfcd-bc08-48e3-9f30-65b6411839c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2de1546b112041f7035a711852140e9bb34babe3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724667"
---
# <a name="ksetupdelkpasswd"></a>ksetup: delkpasswd

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Remove um servidor de senha Kerberos (Kpasswd) para um realm.
## <a name="syntax"></a>Sintaxe
```
ksetup /delkpasswd <RealmName> <KpasswdName>
```
#### <a name="parameters"></a>Parâmetros

|   Parâmetro   |                                                                                                   Descrição                                                                                                   |
|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <RealmName>  |                                O nome do realm é declarado como um nome DNS em maiúsculas, como CORP. CONTOSO.COM e é listado como o realm ou REALm padrão = quando **ksetup** é executado.                                |
| <KpasswdName> | O nome do KDC a ser usado como o servidor de senha Kerberos é declarado como um nome de domínio totalmente qualificado, que não diferencia maiúsculas de minúsculas, como mitkdc.contoso.com. Se o nome do KDC for omitido, o DNS poderá ser usado para localizar KDCs. |

## <a name="remarks"></a>Comentários
Execute o comando **ksetup** para verificar o nome do KDC. Se **kpasswd =** não aparecer na saída, o mapeamento não foi configurado. Vários mapeamentos serão listados, se definido.
## <a name="examples"></a>Exemplos
Verifique o realm CORP. CONTOSO.COM usa o mitkdc.contoso.com do servidor KDC não Windows como o servidor de senha:
```
ksetup /delkpasswd CORP.CONTOSO.COM mitkdc.contoso.com
```
Para verificar se o comando funcionou conforme o esperado, execute **ksetup** no computador com Windows para verificar o realm Corp. CONTOSO.COM não está mapeado para um servidor de senha Kerberos (o nome do KDC).
## <a name="additional-references"></a>Referências adicionais
-   [ksetup](ksetup.md)
-   [ksetup: delkpasswd](ksetup-delkpasswd.md)
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
