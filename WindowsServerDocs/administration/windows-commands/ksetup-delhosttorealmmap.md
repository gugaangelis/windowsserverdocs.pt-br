---
title: 'ksetup: delhosttorealmmap'
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 70b54aaebc0b7b46c34c6f52e45f6583afd6c477
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375155"
---
# <a name="ksetupdelhosttorealmmap"></a>ksetup: delhosttorealmmap



Remove um mapeamento de SPN (nome da entidade de serviço) entre o host declarado e o realm. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /delhosttorealmmap <HostName> <RealmName>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<HostName >|O nome do host é o nome do computador e pode ser declarado como o nome de domínio totalmente qualificado do computador.|
|\<RealmName >|O nome do realm é declarado como um nome DNS em maiúsculas, como CORP. CONTOSO.COM.|

## <a name="remarks"></a>Comentários

Quando um host para o mapeamento de realm (ou vários hosts para território) existirem, esse comando removerá esse mapeamento.

O mapeamento é registrado no registro no **HKEY_LOCAL_MACHINE\SYSTEM\CurrentContolSet\Lsa\Kerberos\HostToRealm**. Você deve verificar o mapeamento no registro depois de usar esse comando.

## <a name="BKMK_Examples"></a>Disso

Alterando a configuração do Realm CONTOSO, exclua o mapeamento do computador host IPops897 para o realm:
```
ksetup /delhosttorealmmap IPops897 CONTOSO
```
Depois de executar esse comando, você pode verificar no registro se o mapeamento está como pretendido.

#### <a name="additional-references"></a>Referências adicionais

-   [Ksetup:addhosttorealmmap](ksetup-addhosttorealmmap.md)
-   [Ksetup](ksetup.md)
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)