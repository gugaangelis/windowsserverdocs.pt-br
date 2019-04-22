---
title: bitsadmin setproxysettings
description: Tópico de comandos do Windows para **setproxysettings bitsadmin** -define as configurações de proxy para o trabalho especificado.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: be8edb1b-614e-4d0b-a8f8-64b4bde3e64b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a3503aab55f5650cb9283ce8a9f1a17359bfd48b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825587"
---
# <a name="bitsadmin-setproxysettings"></a>bitsadmin setproxysettings



Define as configurações de proxy para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetProxySettings <Job> <Usage> [List] [Bypass]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|
|Uso|Um dos seguintes valores:</br>-PRECONFIG — use os padrões do Internet Explorer do proprietário.</br>-NO_PROXY — não use um servidor proxy.</br>– Substituir — use uma lista explícita de proxy e a lista de bypass. Um proxy e a lista de bypass de proxy devem seguir.</br>– Detecção automática — detectar automaticamente as configurações de proxy.|
|List|Usado quando o *uso* parâmetro for definido como substituição — contém uma lista delimitada por vírgulas de servidores proxy para usar.|
|Bypass|Usado quando o *uso* parâmetro for definido como substituição — contém uma lista delimitada por espaço de nomes de host ou endereços IP ou ambos, para o qual as transferências de não devem ser roteada por meio de um proxy. Isso pode ser  **\<local >** para se referir a todos os servidores na mesma LAN. Valores NULL ou "" pode ser usado para obter uma lista de bypass de proxy vazio.|

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir define as configurações de proxy do trabalho nomeado *myDownloadJob*.

```
C:\>bitsadmin /SetProxySettings myDownloadJob PRECONFIG
```

Aqui estão alguns exemplos.

```
bitsadmin /setproxysettings myDownloadJob NO_PROXY
bitsadmin /setproxysettings myDownloadJob OVERRIDE proxy1:80 ""
bitsadmin /setproxysettings myDownloadJob OVERRIDE proxy1,proxy2,proxy3 NULL
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)