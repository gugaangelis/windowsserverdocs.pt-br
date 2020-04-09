---
title: bitsadmin setproxysettings
description: Tópico de comandos do Windows para Bitsadmin setproxysettings, que define as configurações de proxy para o trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: be8edb1b-614e-4d0b-a8f8-64b4bde3e64b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4dea72d956d12070b2638f953a7a00dcb1ed7a9c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849199"
---
# <a name="bitsadmin-setproxysettings"></a>bitsadmin setproxysettings

Define as configurações de proxy para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetProxySettings <Job> <Usage> [List] [Bypass]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Trabalho|O nome de exibição ou o GUID do trabalho|
|Uso|Um dos seguintes valores:</br>-Preconfig – use os padrões do Internet Explorer do proprietário.</br>-NO_PROXY — não use um servidor proxy.</br>-OVERRIDE — use uma lista de proxy explícita e a lista de bypass. É necessário seguir uma lista de proxies de proxy e proxy.</br>-AUTODETECT – detectar automaticamente as configurações de proxy.|
|Lista|Usado quando o parâmetro *Usage* é definido como override — contém uma lista delimitada por vírgulas de servidores proxy a serem usados.|
|Pular|Usado quando o parâmetro *Usage* é definido como override — contém uma lista delimitada por espaço de nomes de host ou endereços IP, ou ambos, para os quais as transferências não devem ser roteadas por meio de um proxy. Isso pode ser **\<> local** para se referir a todos os servidores na mesma LAN. Valores de NULL ou podem ser usados para uma lista de bypass de proxy vazia.|

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir define as configurações de proxy para o trabalho chamado *myDownloadJob*.

```
C:\>bitsadmin /SetProxySettings myDownloadJob PRECONFIG
```

Aqui estão alguns outros exemplos.

```
bitsadmin /setproxysettings myDownloadJob NO_PROXY
bitsadmin /setproxysettings myDownloadJob OVERRIDE proxy1:80 
bitsadmin /setproxysettings myDownloadJob OVERRIDE proxy1,proxy2,proxy3 NULL
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)