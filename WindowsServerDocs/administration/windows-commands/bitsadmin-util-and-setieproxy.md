---
title: SETIEPROXY e bitsadmin util
description: Tópico de comandos do Windows para **util bitsadmin e setieproxy** -definir configurações de proxy a ser usado ao transferir arquivos usando uma conta de serviço.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0e9f31ba-3070-4ffd-a94c-388c8d78f688
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d4f6ab2e52284895d2e7918364c24bbb69f2b1c9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853507"
---
# <a name="bitsadmin-util-and-setieproxy"></a>SETIEPROXY e bitsadmin util

Defina configurações de proxy para usar ao transferir arquivos usando uma conta de serviço.

**BITSAdmin 1.5 e anterior**: Sem suporte.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /Util /SetIEProxy <Account> <Usage>[/Conn <ConnectionName>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Conta|Especifica o tipo de conta de serviço cujas configurações de proxy que você deseja definir. Os valores possíveis são:</br>-   LOCALSYSTEM</br>-NETWORKSERVICE</br>-LOCALSERVICE|
|Uso|Especifica a forma de detecção de proxy para usar. Os valores possíveis são:</br>-NO_PROXY — não use um servidor proxy.</br>– Detecção automática — detectar automaticamente as configurações de proxy.</br>-MANUAL_PROXY — Use uma lista explícita de proxy e a lista de bypass. Especifique a lista de proxy e ignorar a lista imediatamente após a marca de uso. Por exemplo, MANUAL_PROXY proxy1, proxy2 NULL.</br>    -A lista de proxy é uma lista delimitada por vírgulas de servidores proxy para usar.</br>    -O bypass é uma lista delimitada por espaço de nomes de host ou endereços IP ou ambos, para o qual as transferências de não devem ser roteada por meio de um proxy. Isso pode ser \<local > para referir-se a todos os servidores na mesma LAN. Valores NULL ou "" pode ser usado para obter uma lista de bypass de proxy vazio.</br>-AUTOSCRIPT — Mesmo que a detecção automática, exceto que ele também executa um script. Especifique a URL de script, imediatamente após a marca de uso. Por exemplo, AUTOSCRIPT http://server/proxy.js.</br>-REDEFINIÇÃO — Mesmo que NO_PROXY, exceto que ele remove as URLs de proxy manual (se especificado) e as URLs descobertos usando a detecção automática.|
|ConnectionName|Opcional – usado com o **/conn** parâmetro para especificar a conexão de modem para usar. Se você não especificar o **/conn** parâmetro, o BITS usa a conexão de rede local. Especifique o nome de conexão de modem imediatamente após o **/conn** parâmetro.|

## <a name="remarks"></a>Comentários

Cada chamada sucessiva usando essa opção substitui o uso especificado anteriormente, mas não os parâmetros do uso definido anteriormente. Por exemplo, se você especificar NO_PROXY, detecção automática e MANUAL_PROXY em chamadas separadas, o BITS usa o último uso fornecido, mas impede que os parâmetros de uso definido anteriormente.

> [!IMPORTANT]
> Você deve executar esse comando em um prompt de comando elevado para que ela seja concluída com êxito.

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir define o uso de proxy para a conta de serviço de rede.

```
C:\>bitsadmin /Util /SetIEProxy localsystem AUTODETECT
```

Aqui estão mais exemplos.

```
bitsadmin /util /setieproxy localsystem MANUAL_PROXY proxy1,proxy2,proxy3 NULL
bitsadmin /util /setieproxy localsystem MANUAL_PROXY proxy1:80 ""
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)