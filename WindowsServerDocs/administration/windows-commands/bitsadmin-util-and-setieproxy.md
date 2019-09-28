---
title: Bitsadmin util e setieproxy
description: Tópico de comandos do Windows para **Bitsadmin util e setieproxy** -defina as configurações de proxy a serem usadas ao transferir arquivos usando uma conta de serviço.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 9d485c0e9cb135febdb1bf99cec4de08d7c9321b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380217"
---
# <a name="bitsadmin-util-and-setieproxy"></a>Bitsadmin util e setieproxy

Defina as configurações de proxy a serem usadas ao transferir arquivos usando uma conta de serviço.

**BITSAdmin 1,5 e anterior**: Não compatível.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /Util /SetIEProxy <Account> <Usage>[/Conn <ConnectionName>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Conta|Especifica o tipo de conta de serviço cujas configurações de proxy você deseja definir. Os valores possíveis são:</br>-LOCALSYSTEM</br>-NETWORKSERVICE</br>-LOCALSERVICE|
|Uso|Especifica a forma de detecção de proxy a ser usada. Os valores possíveis são:</br>-NO_PROXY — não use um servidor proxy.</br>-AUTODETECT – detecta automaticamente as configurações de proxy.</br>-MANUAL_PROXY – use uma lista de proxy explícita e a lista de bypass. Especifique a lista de proxies e a lista de bypass imediatamente após a marca de uso. Por exemplo, MANUAL_PROXY Proxy1, proxy2 NULL.</br>    -A lista de proxy é uma lista delimitada por vírgula de servidores proxy a serem usados.</br>    -A lista de bypass é uma lista delimitada por espaço de nomes de host ou endereços IP, ou ambos, para os quais as transferências não devem ser roteadas por meio de um proxy. Isso pode ser \<local > para fazer referência a todos os servidores na mesma LAN. Valores de NULL ou "" podem ser usados para uma lista de bypass de proxy vazia.</br>-AutoScript – o mesmo que detecção automática, exceto que também executa um script. Especifique a URL do script imediatamente após a marca de uso. Por exemplo, AutoScript http://server/proxy.js.</br>-RESET — o mesmo que NO_PROXY, exceto pelo fato de remover as URLs de proxy manuais (se especificado) e URLs descobertas usando a detecção automática.|
|ConnectionName|Opcional — usado com o parâmetro **/Conn** para especificar a conexão de modem a ser usada. Se você não especificar o parâmetro **/Conn** , o bits usará a conexão LAN. Especifique o nome da conexão do modem imediatamente após o parâmetro **/Conn** .|

## <a name="remarks"></a>Comentários

Cada chamada sucessiva usando essa opção substitui o uso especificado anteriormente, mas não os parâmetros do uso definido anteriormente. Por exemplo, se você especificar NO_PROXY, AUTODETECT e MANUAL_PROXY em chamadas separadas, o BITS usará o último uso fornecido, mas manterá os parâmetros do uso definido anteriormente.

> [!IMPORTANT]
> Você deve executar esse comando em um prompt de comando elevado para que ele seja concluído com êxito.

## <a name="examples"></a>Exemplos

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

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)