---
title: 'ksetup: delhosttorealmmap'
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3faee482-a96c-4614-86fd-aaa446643ec4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2b6b14785f254a63f0e16fcd16f1cd464a2d69c8
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724690"
---
# <a name="ksetupdelhosttorealmmap"></a>ksetup: delhosttorealmmap



Remove um mapeamento de SPN (nome da entidade de serviço) entre o host declarado e o realm.

## <a name="syntax"></a>Sintaxe

```
ksetup /delhosttorealmmap <HostName> <RealmName>
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<> do nome do host|O nome do host é o nome do computador e pode ser declarado como o nome de domínio totalmente qualificado do computador.|
|\<Realmsname>|O nome do realm é declarado como um nome DNS em maiúsculas, como CORP. CONTOSO.COM.|

## <a name="remarks"></a>Comentários

Quando um host para o mapeamento de realm (ou vários hosts para território) existirem, esse comando removerá esse mapeamento.

O mapeamento é registrado no registro no **HKEY_LOCAL_MACHINE \system\currentcontolset\lsa\kerberos\hosttorealm**. Você deve verificar o mapeamento no registro depois de usar esse comando.

## <a name="examples"></a>Exemplos

Alterando a configuração do Realm CONTOSO, exclua o mapeamento do computador host IPops897 para o realm:
```
ksetup /delhosttorealmmap IPops897 CONTOSO
```
Depois de executar esse comando, você pode verificar no registro se o mapeamento está como pretendido.

## <a name="additional-references"></a>Referências adicionais

-   [Ksetup:addhosttorealmmap](ksetup-addhosttorealmmap.md)
-   [Ksetup](ksetup.md)
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)