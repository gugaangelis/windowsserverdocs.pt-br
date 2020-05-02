---
title: 'ksetup: setreale'
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ab268c40-276b-46ef-ab16-d5ce7667fbed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 453977ac39dd3a52b4f5a3104995f944e4a48392
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724553"
---
# <a name="ksetupsetrealm"></a>ksetup: setreale



Define o nome de um realm Kerberos.

## <a name="syntax"></a>Sintaxe

```
ksetup /setrealm <DNSDomainName>
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<NomeDomínioDNS>|O nome de domínio DNS pode estar na forma de um nome de domínio totalmente qualificado ou um nome de domínio simples.|

## <a name="remarks"></a>Comentários

O parâmetro de nome de domínio DNS deve ser inserido em letras maiúsculas. Caso contrário, o comando **ksetup** solicitará a verificação para continuar.

Não há suporte para a definição do realm Kerberos em um controlador de domínio. Tentar fazer isso causará um aviso e uma falha de comando.

## <a name="examples"></a>Exemplos

Defina o realm para este computador como um nome de domínio específico para restringir o acesso por um controlador que não seja de domínio apenas para o realm Kerberos da CONTOSO:
```
ksetup /setrealm CONTOSO
```

## <a name="additional-references"></a>Referências adicionais

-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Ksetup](ksetup.md)
-   [Ksetup:removerealm](ksetup-removerealm.md)