---
title: ksetup:addkpasswd
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d3196995-1b38-48ff-ba08-911cfab77317
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2a85eb6dfe30c33126504744a7659fe2cc573087
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856817"
---
# <a name="ksetupaddkpasswd"></a>ksetup:addkpasswd



Adiciona um endereço de servidor de senha (Kpasswd) do Kerberos para um território. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /addkpasswd <RealmName> [<KpasswdName>]
```

### <a name="parameters"></a>Parâmetros

Se o realm Kerberos que a estação de trabalho estará autenticando para oferece suporte a Kerberos alterar o protocolo de senha, você pode configurar um computador cliente executando o sistema operacional Windows para usar um servidor de senha Kerberos. Essa configuração é definida no lado do território.

|Parâmetro|Descrição|
|---------|-----------|
|\<RealmName>|O nome do território é declarado como um nome DNS de letras maiusculas, como CORP. CONTOSO.COM e é listado como o padrão realm ou território = quando **ksetup** é executado.|
|\<KpasswdName>|O nome do KDC que deve ser usado como o servidor de senha Kerberos é declarado como um nome de domínio totalmente qualificado diferencia maiusculas de minúsculas, como mitkdc.microsoft.com. Se o nome do KDC é omitido, o DNS pode ser usado para localizar os KDCs.|

## <a name="remarks"></a>Comentários

Se o realm Kerberos que a estação de trabalho estará autenticando para oferece suporte a Kerberos alterar o protocolo de senha, você pode configurar um computador cliente executando o sistema operacional Windows para usar um servidor de senha Kerberos.

Execute o comando **ksetup** para verificar o nome do KDC. Se **kpasswd =** não aparecer na saída, o mapeamento não foi configurado.

Você pode adicionar nomes adicionais de KDC, um por vez.

## <a name="BKMK_Examples"></a>Exemplos

Configurar o realm, CORP. CONTOSO.COM, para que ele usa o servidor não - Windows KDC, mitkdc.contoso.com, como o servidor de senha:
```
ksetup /addkpasswd CORP.CONTOSO.COM mitkdc.contoso.com
```
Isso resulta em um servidor de senha Kerberos não - Windows que controla todas as senhas para autenticação entre ele e o realm.

#### <a name="additional-references"></a>Referências adicionais

-   [Ksetup](ksetup.md)
-   [Ksetup:delkpasswd](ksetup-delkpasswd.md)
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)