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
ms.openlocfilehash: 3f62208d6576890529be80b1c6cb3cc073a2b4e6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853357"
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

Por engano, defina o nome de realm por erro de ortografia ". COM? no computador local para CORP. CONTOSO. CON
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
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)