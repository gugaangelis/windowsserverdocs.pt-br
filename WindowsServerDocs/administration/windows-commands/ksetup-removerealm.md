---
title: 'ksetup: removerealm'
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 11858d8a24d4f125c83b3e4092ac48f336a9ef0b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374950"
---
# <a name="ksetupremoverealm"></a>ksetup: removerealm



Exclui todas as informações do realm especificado do registro. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /removerealm <RealmName>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<RealmName >|O nome do realm é declarado como um nome DNS em maiúsculas, como CORP. CONTOSO.COM, e ele é listado como o realm padrão quando **ksetup** é executado.|

## <a name="remarks"></a>Comentários

O nome do realm é armazenado em dois locais no registro: **HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001** e **\CurrentControlSet\Control\Lsa\Kerberos**.

Não é possível remover o nome de realm padrão do controlador de domínio porque isso redefinirá suas informações de DNS e removê-lo poderá tornar o controlador de domínio inutilizável.

## <a name="BKMK_Examples"></a>Disso

Definido incorretamente o nome do Realm digitando ". COM" com erro de ortografia no computador local para CORP. Funcionam. TELEFONEMA
```
ksetup /setrealm CORP.CONTOSO.CON
```
Remova esse nome de realm errado do computador local:
```
ksetup /removerealm CORP.CONTOSO.CON
```
Verifique a remoção executando **ksetup** e examine a saída.

#### <a name="additional-references"></a>Referências adicionais

-   [Ksetup](ksetup.md)
-   [Ksetup:setrealm](ksetup-setrealm.md)
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)