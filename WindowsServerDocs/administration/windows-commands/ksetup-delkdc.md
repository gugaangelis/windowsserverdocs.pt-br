---
title: 'ksetup: delkdc'
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: b918f8c2fa0ec09c2aae77517a0ee2c9e77ce2dc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375143"
---
# <a name="ksetupdelkdc"></a>ksetup: delkdc



Exclui instâncias de nomes de centro de distribuição de chaves (KDC) para o realm Kerberos. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /delkdc <RealmName> <KDCName>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<RealmName >|O nome do realm é declarado como um nome DNS em maiúsculas, como CORP. CONTOSO.COM, e ele é listado como o realm padrão quando **ksetup** é executado. É nesse realm que você está tentando excluir o outro KDC.|
|\<KDCName >|O nome do KDC é declarado como um nome de domínio totalmente qualificado, que não diferencia maiúsculas de minúsculas, como mitkdc.contoso.com.|

## <a name="remarks"></a>Comentários

Esses mapeamentos são armazenados no registro no **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\LSA\Kerberos\Domains**. Para remover dados de configuração de realm de vários computadores, use o snap-in de modelo de configuração de segurança e a distribuição de política em vez de usar **ksetup** explicitamente em computadores individuais.

Em computadores que executam o Windows 2000 Server com Service Pack 1 (SP1) e anterior, o computador deve ser reiniciado antes que a configuração de configuração de realm seja usada.

Para verificar o nome de realm padrão do computador, ou para verificar se esse comando funcionou conforme o esperado, execute **ksetup** no prompt de comando e verifique se o KDC removido não existe na lista.

## <a name="BKMK_Examples"></a>Disso

Os requisitos de segurança para este computador foram alterados, portanto, o vínculo entre o realm do Windows e o realm não Windows deve ser removido. Primeiro, determine qual associação remover e produzir a saída de associações existentes:
```
ksetup
```
Remova a Associação usando o seguinte comando:
```
Ksetup /delkdc CORP.CONTOSO.COM mitkdc.contoso.com
```

#### <a name="additional-references"></a>Referências adicionais

-   [Ksetup](ksetup.md)
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)