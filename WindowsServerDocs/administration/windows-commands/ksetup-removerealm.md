---
title: 'ksetup: removerealm'
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 39f0c6f0-4c50-4781-941e-0893495405e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1465ce08c0cf45de828683324b29fb2df8d0e893
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841449"
---
# <a name="ksetupremoverealm"></a>ksetup: removerealm



Exclui todas as informações do realm especificado do registro. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /removerealm <RealmName>
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Realmsname >|O nome do realm é declarado como um nome DNS em maiúsculas, como CORP. CONTOSO.COM, e ele é listado como o realm padrão quando **ksetup** é executado.|

## <a name="remarks"></a>Comentários

O nome do realm é armazenado em dois locais no registro: **HKEY_LOCAL_MACHINE \system\controlset001** e **\CurrentControlSet\Control\Lsa\Kerberos**.

Não é possível remover o nome de realm padrão do controlador de domínio porque isso redefinirá suas informações de DNS e removê-lo poderá tornar o controlador de domínio inutilizável.

## <a name="examples"></a><a name=BKMK_Examples></a>Disso

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