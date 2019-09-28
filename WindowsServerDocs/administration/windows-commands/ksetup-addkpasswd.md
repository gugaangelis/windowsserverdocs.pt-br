---
title: 'ksetup: addkpasswd'
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 72c27cb6b068dc46cd58e753b4b08d68b39bfb20
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375187"
---
# <a name="ksetupaddkpasswd"></a>ksetup: addkpasswd



Adiciona um endereço de servidor de senha Kerberos (Kpasswd) para um realm. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /addkpasswd <RealmName> [<KpasswdName>]
```

### <a name="parameters"></a>Parâmetros

Se o realm Kerberos que a estação de trabalho estará Autenticando para dar suporte ao protocolo de alteração de senha Kerberos, você poderá configurar um computador cliente executando o sistema operacional Windows para usar um servidor de senha Kerberos. Essa configuração é definida no lado do território.

|Parâmetro|Descrição|
|---------|-----------|
|\<RealmName >|O nome do realm é declarado como um nome DNS em maiúsculas, como CORP. CONTOSO.COM e é listado como o realm ou Realm padrão = quando **ksetup** é executado.|
|\<KpasswdName >|O nome do KDC que deve ser usado como o servidor de senha Kerberos é declarado como um nome de domínio totalmente qualificado que não diferencia maiúsculas de minúsculas, como mitkdc.microsoft.com. Se o nome do KDC for omitido, o DNS poderá ser usado para localizar KDCs.|

## <a name="remarks"></a>Comentários

Se o realm Kerberos que a estação de trabalho estará Autenticando para dar suporte ao protocolo de alteração de senha Kerberos, você poderá configurar um computador cliente executando o sistema operacional Windows para usar um servidor de senha Kerberos.

Execute o comando **ksetup** para verificar o nome do KDC. Se **kpasswd =** não aparecer na saída, o mapeamento não foi configurado.

Você pode adicionar mais nomes KDC um de cada vez.

## <a name="BKMK_Examples"></a>Disso

Configurar o realm, CORP. CONTOSO.COM, para que ele use o servidor KDC não Windows, mitkdc.contoso.com, como o servidor de senha:
```
ksetup /addkpasswd CORP.CONTOSO.COM mitkdc.contoso.com
```
Isso resulta em um servidor de senha Kerberos não Windows que controla todas as senhas para autenticação entre ela e o realm.

#### <a name="additional-references"></a>Referências adicionais

-   [Ksetup](ksetup.md)
-   [Ksetup:delkpasswd](ksetup-delkpasswd.md)
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)