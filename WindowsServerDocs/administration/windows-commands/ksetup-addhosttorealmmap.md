---
title: ksetup:addhosttorealmmap
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 25cf258309c94f0efde980018dd5dcf3c7df4d60
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837497"
---
# <a name="ksetupaddhosttorealmmap"></a>ksetup:addhosttorealmmap



Adiciona um mapeamento de nome da entidade (SPN) do serviço entre o host indicado e o realm. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /addhosttorealmmap <HostName> <RealmName>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<HostName>|O nome do host é o nome do computador e ele pode ser declarado como nome de domínio totalmente qualificado do computador.|
|\<RealmName>|O nome do território é declarado como um nome DNS de letras maiusculas, como CORP. CONTOSO.COM.|

## <a name="remarks"></a>Comentários

Esse comando permite que você mapeie um host ou em vários hosts que estão compartilhando o mesmo sufixo DNS para o realm.

O mapeamento é registrado no registro em **HKEY_LOCAL_MACHINE\SYSTEM\CurrentContolSet\Lsa\Kerberos\HostToRealm**.

## <a name="BKMK_Examples"></a>Exemplos

Como parte de Configurando o realm CONTOSO, mapear IPops897 computador host para o realm:
```
ksetup /addhosttorealmmap IPops897 CONTOSO
```
Verifique se que o mapeamento é conforme o esperado no registro.

#### <a name="additional-references"></a>Referências adicionais

-   [Ksetup:delhosttorealmmap](ksetup-delhosttorealmmap.md)
-   [Ksetup](ksetup.md)
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)