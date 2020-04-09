---
title: 'ksetup: delhosttorealmmap'
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3faee482-a96c-4614-86fd-aaa446643ec4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 85e9f6b4a9f1c9050ed843f3837a2bd87aaf6eae
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841729"
---
# <a name="ksetupdelhosttorealmmap"></a>ksetup: delhosttorealmmap



Remove um mapeamento de SPN (nome da entidade de serviço) entre o host declarado e o realm. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /delhosttorealmmap <HostName> <RealmName>
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|> de nome de host \<|O nome do host é o nome do computador e pode ser declarado como o nome de domínio totalmente qualificado do computador.|
|\<Realmsname >|O nome do realm é declarado como um nome DNS em maiúsculas, como CORP. CONTOSO.COM.|

## <a name="remarks"></a>Comentários

Quando um host para o mapeamento de realm (ou vários hosts para território) existirem, esse comando removerá esse mapeamento.

O mapeamento é registrado no registro no **HKEY_LOCAL_MACHINE \system\currentcontolset\lsa\kerberos\hosttorealm**. Você deve verificar o mapeamento no registro depois de usar esse comando.

## <a name="examples"></a><a name=BKMK_Examples></a>Disso

Alterando a configuração do Realm CONTOSO, exclua o mapeamento do computador host IPops897 para o realm:
```
ksetup /delhosttorealmmap IPops897 CONTOSO
```
Depois de executar esse comando, você pode verificar no registro se o mapeamento está como pretendido.

## <a name="additional-references"></a>Referências adicionais

-   [Ksetup:addhosttorealmmap](ksetup-addhosttorealmmap.md)
-   [Ksetup](ksetup.md)
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)