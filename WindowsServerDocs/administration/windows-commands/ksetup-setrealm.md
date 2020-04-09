---
title: 'ksetup: setreale'
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ab268c40-276b-46ef-ab16-d5ce7667fbed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: acdbfaabe341c8efb19c6e9d183022375f679de7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841309"
---
# <a name="ksetupsetrealm"></a>ksetup: setreale



Define o nome de um realm Kerberos. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /setrealm <DNSDomainName>
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|> \<NomeDomínioDNS|O nome de domínio DNS pode estar na forma de um nome de domínio totalmente qualificado ou um nome de domínio simples.|

## <a name="remarks"></a>Comentários

O parâmetro de nome de domínio DNS deve ser inserido em letras maiúsculas. Caso contrário, o comando **ksetup** solicitará a verificação para continuar.

Não há suporte para a definição do realm Kerberos em um controlador de domínio. Tentar fazer isso causará um aviso e uma falha de comando.

## <a name="examples"></a><a name=BKMK_Examples></a>Disso

Defina o realm para este computador como um nome de domínio específico para restringir o acesso por um controlador que não seja de domínio apenas para o realm Kerberos da CONTOSO:
```
ksetup /setrealm CONTOSO
```

## <a name="additional-references"></a>Referências adicionais

-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Ksetup](ksetup.md)
-   [Ksetup:removerealm](ksetup-removerealm.md)