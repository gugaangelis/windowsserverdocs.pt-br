---
title: 'ksetup: addhosttorealmmap'
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 237742d5-fa68-466c-b97e-636f489248ea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e6a28c6001707fac245de7136b5fb5bd38495027
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375655"
---
# <a name="ksetupaddhosttorealmmap"></a>ksetup: addhosttorealmmap



Adiciona um mapeamento de SPN (nome da entidade de serviço) entre o host declarado e o realm. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /addhosttorealmmap <HostName> <RealmName>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<HostName >|O nome do host é o nome do computador e pode ser declarado como o nome de domínio totalmente qualificado do computador.|
|\<RealmName >|O nome do realm é declarado como um nome DNS em maiúsculas, como CORP. CONTOSO.COM.|

## <a name="remarks"></a>Comentários

Esse comando permite mapear um host ou vários hosts que estão compartilhando o mesmo sufixo DNS para o realm.

O mapeamento é registrado no registro no **HKEY_LOCAL_MACHINE\SYSTEM\CurrentContolSet\Lsa\Kerberos\HostToRealm**.

## <a name="BKMK_Examples"></a>Disso

Como parte da configuração do Realm CONTOSO, mapeie o computador host IPops897 para o realm:
```
ksetup /addhosttorealmmap IPops897 CONTOSO
```
Verifique no registro se o mapeamento é o pretendido.

#### <a name="additional-references"></a>Referências adicionais

-   [Ksetup:delhosttorealmmap](ksetup-delhosttorealmmap.md)
-   [Ksetup](ksetup.md)
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)