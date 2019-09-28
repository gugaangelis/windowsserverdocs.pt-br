---
title: 'ksetup: setreale'
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ab268c40-276b-46ef-ab16-d5ce7667fbed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1bbe5c000b7e84066c19511639fe3d92d7e4b558
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374905"
---
# <a name="ksetupsetrealm"></a>ksetup: setreale



Define o nome de um realm Kerberos. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /setrealm <DNSDomainName>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<DNSDomainName >|O nome de domínio DNS pode estar na forma de um nome de domínio totalmente qualificado ou um nome de domínio simples.|

## <a name="remarks"></a>Comentários

O parâmetro de nome de domínio DNS deve ser inserido em letras maiúsculas. Caso contrário, o comando **ksetup** solicitará a verificação para continuar.

Não há suporte para a definição do realm Kerberos em um controlador de domínio. Tentar fazer isso causará um aviso e uma falha de comando.

## <a name="BKMK_Examples"></a>Disso

Defina o realm para este computador como um nome de domínio específico para restringir o acesso por um controlador que não seja de domínio apenas para o realm Kerberos da CONTOSO:
```
ksetup /setrealm CONTOSO
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Ksetup](ksetup.md)
-   [Ksetup:removerealm](ksetup-removerealm.md)