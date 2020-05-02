---
title: 'ksetup: addkdc'
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 98bfc23a-14c4-401c-bcb3-9903c5cdde64
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 76d592e4f1c32305d6f939a66a6ad42cd582b032
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724761"
---
# <a name="ksetupaddkdc"></a>ksetup: addkdc



Adiciona um endereço centro de distribuição de chaves (KDC) para o realm Kerberos especificado.

## <a name="syntax"></a>Sintaxe

```
ksetup /addkdc <RealmName> [<KDCName>] 
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Realmsname>|O nome do realm é declarado como um nome DNS em maiúsculas, como CORP. CONTOSO.COM, e ele é listado como o realm padrão quando **ksetup** é executado. É nesse realm que você está tentando adicionar o outro KDC.|
|\<> KDCName|O nome do KDC é declarado como um nome de domínio totalmente qualificado que não diferencia maiúsculas de minúsculas, como mitkdc.microsoft.com. Se o nome do KDC for omitido, o DNS localizará KDCs.|

## <a name="remarks"></a>Comentários

Esses mapeamentos são armazenados no registro em **HKEY_LOCAL_MACHINE \system\currentcontrolset\control\lsa\kerberos\domains**. Para implantar dados de configuração de realm Kerberos em vários computadores, use o snap-in de modelo de configuração de segurança e a distribuição de política em vez de usar **ksetup** explicitamente em computadores individuais.

O computador deve ser reiniciado para que a nova configuração de realm seja usada.

Para verificar o nome de realm padrão do computador ou para verificar se esse comando funcionou conforme o esperado, execute **ksetup** no prompt de comando e verifique a saída do KDC adicionado.

## <a name="examples"></a>Exemplos

Configure um servidor KDC não Windows e o realm que a estação de trabalho deve usar:
```
ksetup /addkdc CORP.CONTOSO.COM mitkdc.contoso.com
```
Execute a ferramenta Ksetup na linha de comando do mesmo computador que o comando anterior para definir a senha da conta do computador local como p@sswrd1%. Em seguida, reinicie o computador.
```
Ksetup /setcomputerpassword p@sswrd1%
```

## <a name="additional-references"></a>Referências adicionais

-   [Ksetup](ksetup.md)
-   [Ksetup:setcomputerpassword](ksetup-setcomputerpassword.md)
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)