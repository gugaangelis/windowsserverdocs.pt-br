---
title: 'ksetup: removerealm'
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 39f0c6f0-4c50-4781-941e-0893495405e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bb7bf4663594a6c164d6495a9ba4cd81942afb79
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724601"
---
# <a name="ksetupremoverealm"></a>ksetup: removerealm



Exclui todas as informações do realm especificado do registro.

## <a name="syntax"></a>Sintaxe

```
ksetup /removerealm <RealmName>
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Realmsname>|O nome do realm é declarado como um nome DNS em maiúsculas, como CORP. CONTOSO.COM, e ele é listado como o realm padrão quando **ksetup** é executado.|

## <a name="remarks"></a>Comentários

O nome do realm é armazenado em dois locais no registro: **HKEY_LOCAL_MACHINE \system\controlset001** e **\CurrentControlSet\Control\Lsa\Kerberos**.

Não é possível remover o nome de realm padrão do controlador de domínio porque isso redefinirá suas informações de DNS e removê-lo poderá tornar o controlador de domínio inutilizável.

## <a name="examples"></a>Exemplos

Definido incorretamente o nome do Realm com grafia incorreta. COM no computador local para CORP. Funcionam. TELEFONEMA
```
ksetup /setrealm CORP.CONTOSO.CON
```
Remova esse nome de realm errado do computador local:
```
ksetup /removerealm CORP.CONTOSO.CON
```
Verifique a remoção executando **ksetup** e examine a saída.

## <a name="additional-references"></a>Referências adicionais

-   [Ksetup](ksetup.md)
-   [Ksetup:setrealm](ksetup-setrealm.md)
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)