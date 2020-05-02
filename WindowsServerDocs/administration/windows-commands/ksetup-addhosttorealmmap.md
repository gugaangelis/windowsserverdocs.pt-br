---
title: 'ksetup: addhosttorealmmap'
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 237742d5-fa68-466c-b97e-636f489248ea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 732dccc868ca85b108ba443d912788a14dd0e107
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724769"
---
# <a name="ksetupaddhosttorealmmap"></a>ksetup: addhosttorealmmap



Adiciona um mapeamento de SPN (nome da entidade de serviço) entre o host declarado e o realm.

## <a name="syntax"></a>Sintaxe

```
ksetup /addhosttorealmmap <HostName> <RealmName>
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<> do nome do host|O nome do host é o nome do computador e pode ser declarado como o nome de domínio totalmente qualificado do computador.|
|\<Realmsname>|O nome do realm é declarado como um nome DNS em maiúsculas, como CORP. CONTOSO.COM.|

## <a name="remarks"></a>Comentários

Esse comando permite mapear um host ou vários hosts que estão compartilhando o mesmo sufixo DNS para o realm.

O mapeamento é registrado no registro no **HKEY_LOCAL_MACHINE \system\currentcontolset\lsa\kerberos\hosttorealm**.

## <a name="examples"></a>Exemplos

Como parte da configuração do Realm CONTOSO, mapeie o computador host IPops897 para o realm:
```
ksetup /addhosttorealmmap IPops897 CONTOSO
```
Verifique no registro se o mapeamento é o pretendido.

## <a name="additional-references"></a>Referências adicionais

-   [Ksetup:delhosttorealmmap](ksetup-delhosttorealmmap.md)
-   [Ksetup](ksetup.md)
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)