---
title: ksetup:setrealm
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: aa6b2a21904ec4dae1e60def5bd36647291b1af6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877397"
---
# <a name="ksetupsetrealm"></a>ksetup:setrealm



Define o nome do realm do Kerberos. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /setrealm <DNSDomainName>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<DNSDomainName>|O nome de domínio do DNS pode ser na forma de um nome de domínio totalmente qualificado ou nome de domínio simples.|

## <a name="remarks"></a>Comentários

O parâmetro de nome de domínio DNS deve ser inserido em letras maiusculas. Caso contrário, o **ksetup** comando solicitará a verificação continuar.

Não há suporte para definir o realm do Kerberos em um controlador de domínio. Tentar fazer isso fará com que um aviso e uma falha de comando.

## <a name="BKMK_Examples"></a>Exemplos

Defina o realm para este computador como um nome de domínio específico para restringir o acesso por um controlador de domínio não apenas para o realm do Kerberos da CONTOSO:
```
ksetup /setrealm CONTOSO
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
-   [Ksetup](ksetup.md)
-   [Ksetup:removerealm](ksetup-removerealm.md)