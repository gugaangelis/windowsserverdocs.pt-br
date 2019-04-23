---
title: ksetup:delkpasswd
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2db0bfcd-bc08-48e3-9f30-65b6411839c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e487389e0f27f58aacaea2b81d573dc9a965e42f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889077"
---
# <a name="ksetupdelkpasswd"></a>ksetup:delkpasswd

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Remove um servidor de senha Kerberos (Kpasswd) para um território. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).
## <a name="syntax"></a>Sintaxe
```
ksetup /delkpasswd <RealmName> <KpasswdName>
```
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|<RealmName>|O nome do território é declarado como um nome DNS de letras maiusculas, como CORP. CONTOSO.COM e é listado como o padrão realm ou TERRITÓRIO = quando **ksetup** é executado.|
|<KpasswdName>|O nome a ser usado como o servidor de senha Kerberos do KDC é declarado como um nome de domínio totalmente qualificado, diferencia maiusculas de minúsculas, como mitkdc.contoso.com. Se o nome do KDC é omitido, o DNS pode ser usado para localizar os KDCs.|
## <a name="remarks"></a>Comentários
Execute o comando **ksetup** para verificar o nome do KDC. Se **kpasswd =** não aparecer na saída, em seguida, o mapeamento não foi configurado. Vários mapeamentos serão listados, se definido.
## <a name="BKMK_Examples"></a>Exemplos
Verifique se o realm CORP. CONTOSO.COM usa o KDC não - Windows server mitkdc.contoso.com como o servidor de senha:
```
ksetup /delkpasswd CORP.CONTOSO.COM mitkdc.contoso.com
```
Para verificar se o comando funcionou conforme o esperado, execute **ksetup** no computador do Windows para verificar o realm CORP. CONTOSO.COM não está mapeado para um servidor de senha (o nome do KDC) Kerberos.
## <a name="additional-references"></a>Referências adicionais
-   [ksetup](ksetup.md)
-   [ksetup:delkpasswd](ksetup-delkpasswd.md)
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
