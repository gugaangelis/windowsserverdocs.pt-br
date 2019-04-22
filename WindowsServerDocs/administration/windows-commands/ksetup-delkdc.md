---
title: ksetup:delkdc
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7d6ec389-094c-4a7b-a78b-605497ddc289
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6e2fa065ca60338b04cc8718e199805cc494c908
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814557"
---
# <a name="ksetupdelkdc"></a>ksetup:delkdc



Exclui instâncias de nomes do Centro de distribuição de chaves (KDC) para o realm do Kerberos. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /delkdc <RealmName> <KDCName>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<RealmName>|O nome do território é declarado como um nome DNS de letras maiusculas, como CORP. CONTOSO.COM e ele é listado como o realm padrão quando **ksetup** é executado. É esse realm da qual você está tentando excluir outro KDC.|
|\<KDCName>|O nome do KDC é declarado como um nome de domínio totalmente qualificado, diferencia maiusculas de minúsculas, como mitkdc.contoso.com.|

## <a name="remarks"></a>Comentários

Esses mapeamentos são armazenados no registro em **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\LSA\Kerberos\Domains**. Para remover dados de configuração do realm de vários computadores, use a distribuição de snap-in e a política de modelo de configuração de segurança em vez de usar **ksetup** explicitamente em computadores individuais.

Em computadores que executam o Windows 2000 Server com Service Pack 1 (SP1) e versões anteriores, o computador deve ser reiniciado antes que a configuração alterada realm será usada.

Para verificar o nome de realm padrão para o computador, ou para verificar se esse comando funcionou conforme o esperado, execute **ksetup** no prompt de comando e verifique se que o KDC que foi removido não existe na lista.

## <a name="BKMK_Examples"></a>Exemplos

Os requisitos de segurança para este computador foram alterados, portanto, o link entre o território do Windows e o realm de não-Windows deve ser removido. Primeiro, determine qual a associação para remover e produzir a saída das associações existentes:
```
ksetup
```
Remova a associação usando o comando a seguir:
```
Ksetup /delkdc CORP.CONTOSO.COM mitkdc.contoso.com
```

#### <a name="additional-references"></a>Referências adicionais

-   [Ksetup](ksetup.md)
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)