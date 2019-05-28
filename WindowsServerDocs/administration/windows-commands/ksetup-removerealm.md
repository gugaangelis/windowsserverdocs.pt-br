---
title: ksetup:removerealm
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 39f0c6f0-4c50-4781-941e-0893495405e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 579b0772e4642389b90aa370dad80a3eebea9d34
ms.sourcegitcommit: 08eba714d3ceb5f2dfb5486d6b990da1aa4dcbdd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65564716"
---
# <a name="ksetupremoverealm"></a>ksetup:removerealm



Exclui todas as informações para o realm especificado do registro. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /removerealm <RealmName>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<RealmName>|O nome do território é declarado como um nome DNS de letras maiusculas, como CORP. CONTOSO.COM e ele é listado como o realm padrão quando **ksetup** é executado.|

## <a name="remarks"></a>Comentários

O nome do território é armazenado em dois locais no registro: **HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001** e **\CurrentControlSet\Control\Lsa\Kerberos**.

Não é possível remover o nome de realm padrão do controlador de domínio, porque isso redefinirá a suas informações de DNS e removê-la pode inutilizar o controlador de domínio.

## <a name="BKMK_Examples"></a>Exemplos

Por engano, defina o nome de realm errando ".COM" no computador local para CORP. CONTOSO. CON
```
ksetup /setrealm CORP.CONTOSO.CON
```
Remova esse nome de realm incorretos do computador local:
```
ksetup /removerealm CORP.CONTOSO.CON
```
Verifique se a remoção, executando **ksetup** e examine a saída.

#### <a name="additional-references"></a>Referências adicionais

-   [Ksetup](ksetup.md)
-   [Ksetup:setrealm](ksetup-setrealm.md)
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)