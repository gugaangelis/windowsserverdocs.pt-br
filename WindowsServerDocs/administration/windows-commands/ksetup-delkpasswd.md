---
title: 'ksetup: delkpasswd'
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2db0bfcd-bc08-48e3-9f30-65b6411839c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b849265e6036f338413b75fe1da2067e4cdb4cd8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841649"
---
# <a name="ksetupdelkpasswd"></a>ksetup: delkpasswd

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Remove um servidor de senha Kerberos (Kpasswd) para um realm. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).
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
## <a name="examples"></a><a name=BKMK_Examples></a>Disso
Verifique o realm CORP. CONTOSO.COM usa o mitkdc.contoso.com do servidor KDC não Windows como o servidor de senha:
```
ksetup /delkpasswd CORP.CONTOSO.COM mitkdc.contoso.com
```
Para verificar se o comando funcionou conforme o esperado, execute **ksetup** no computador com Windows para verificar o realm Corp. CONTOSO.COM não está mapeado para um servidor de senha Kerberos (o nome do KDC).
## <a name="additional-references"></a>Referências adicionais
-   [ksetup](ksetup.md)
-   [ksetup: delkpasswd](ksetup-delkpasswd.md)
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
