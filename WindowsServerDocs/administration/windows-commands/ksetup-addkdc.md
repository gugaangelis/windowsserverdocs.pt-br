---
title: ksetup:addkdc
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 98bfc23a-14c4-401c-bcb3-9903c5cdde64
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d3e0d38bdec11618561ee4acaa32ffdd06695fab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868527"
---
# <a name="ksetupaddkdc"></a>ksetup:addkdc



Adiciona um endereço do Centro de distribuição de chaves (KDC) para o realm do Kerberos fornecido. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /addkdc <RealmName> [<KDCName>] 
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<RealmName>|O nome do território é declarado como um nome DNS de letras maiusculas, como CORP. CONTOSO.COM e ele é listado como o realm padrão quando **ksetup** é executado. É esse realm que você está tentando adicionar outro KDC.|
|\<KDCName>|O nome do KDC é declarado como um nome de domínio totalmente qualificado diferencia maiusculas de minúsculas, como mitkdc.microsoft.com. Se o nome do KDC é omitido, o DNS localizará os KDCs.|

## <a name="remarks"></a>Comentários

Esses mapeamentos são armazenados no registro em **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\LSA\Kerberos\Domains**. Para implantar dados de configuração de realm do Kerberos em vários computadores, use a distribuição de snap-in e a política de modelo de configuração de segurança em vez de usar **ksetup** explicitamente em computadores individuais.

O computador deve ser reiniciado antes que a nova configuração de realm será usada.

Para verificar o nome de realm padrão para o computador, ou para verificar se esse comando funcionou conforme o esperado, execute **ksetup** no prompt de comando e verifique se a saída para o KDC adicionado.

## <a name="BKMK_Examples"></a>Exemplos

Configure um servidor KDC não - Windows e o realm do que a estação de trabalho deve usar:
```
ksetup /addkdc CORP.CONTOSO.COM mitkdc.contoso.com
```
Execute a ferramenta de Ksetup na linha de comando do mesmo computador como no comando anterior para definir a senha da conta de computador local como "p@sswrd1%?. Em seguida, reinicie o computador.
```
Ksetup /setcomputerpassword p@sswrd1%
```

#### <a name="additional-references"></a>Referências adicionais

-   [Ksetup](ksetup.md)
-   [Ksetup:setcomputerpassword](ksetup-setcomputerpassword.md)
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)