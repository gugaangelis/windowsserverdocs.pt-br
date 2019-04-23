---
title: ksetup:delhosttorealmmap
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3faee482-a96c-4614-86fd-aaa446643ec4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cf01edc4932fd5ec1cf98043de04286b3a100a34
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882337"
---
# <a name="ksetupdelhosttorealmmap"></a>ksetup:delhosttorealmmap



Remove um mapeamento de nome da entidade (SPN) do serviço entre o host indicado e o realm. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /delhosttorealmmap <HostName> <RealmName>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<HostName>|O nome do host é o nome do computador e ele pode ser declarado como nome de domínio totalmente qualificado do computador.|
|\<RealmName>|O nome do território é declarado como um nome DNS de letras maiusculas, como CORP. CONTOSO.COM.|

## <a name="remarks"></a>Comentários

Quando um host para o realm (ou vários hosts ao realm) de mapeamento existe, este comando remove esse mapeamento.

O mapeamento é registrado no registro em **HKEY_LOCAL_MACHINE\SYSTEM\CurrentContolSet\Lsa\Kerberos\HostToRealm**. Você deve verificar o mapeamento no registro depois de usar esse comando.

## <a name="BKMK_Examples"></a>Exemplos

Alterando a configuração do realm CONTOSO, exclua o mapeamento do computador host IPops897 para o realm:
```
ksetup /delhosttorealmmap IPops897 CONTOSO
```
Depois de executar esse comando, você pode verificar no registro que o mapeamento é conforme o esperado.

#### <a name="additional-references"></a>Referências adicionais

-   [Ksetup:addhosttorealmmap](ksetup-addhosttorealmmap.md)
-   [Ksetup](ksetup.md)
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)