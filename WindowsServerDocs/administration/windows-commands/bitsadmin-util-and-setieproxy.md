---
title: bitsadmin util e setieproxy
description: Artigo de referência para o comando Bitsadmin util e setieproxy, que define as configurações de proxy a serem usadas ao transferir arquivos usando uma conta de serviço.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0e9f31ba-3070-4ffd-a94c-388c8d78f688
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 018e9400dd2463b61f053d37338740090670f51a
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927318"
---
# <a name="bitsadmin-util-and-setieproxy"></a>bitsadmin util e setieproxy

Defina as configurações de proxy a serem usadas ao transferir arquivos usando uma conta de serviço. Você deve executar esse comando em um prompt de comando elevado para que ele seja concluído com êxito.

> [!NOTE]
> Esse comando não tem suporte no BITS 1,5 e versões anteriores.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /util /setieproxy <account> <usage> [/conn <connectionname>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ---------- |
| account | Especifica a conta de serviço cujas configurações de proxy você deseja definir. Os valores possíveis incluem:<ul><li>LOCALSYSTEM</li><li>   NETWORKSERVICE</li><li>LocalService.</li></ul> |
| uso | Especifica a forma de detecção de proxy a ser usada. Os valores possíveis incluem:<ul><li>**NO_PROXY.** Não use um servidor proxy.</li><li>**Detecção automática.** Detectar automaticamente as configurações de proxy.</li><li>**MANUAL_PROXY.** Use uma lista de proxies especificada e a lista de bypass. Você deve especificar suas listas imediatamente após a marca de uso. Por exemplo, `MANUAL_PROXY proxy1,proxy2 NULL`.<ul><li>**Lista de proxies.** Uma lista delimitada por vírgulas de servidores proxy a serem usados.</li><li>**Lista de bypass.** Uma lista delimitada por espaço de nomes de host ou endereços IP, ou ambos, para os quais as transferências não devem ser roteadas por meio de um proxy. Isso pode ser \<local> referente a todos os servidores na mesma LAN. Valores de NULL ou podem ser usados para uma lista de bypass de proxy vazia.</li></ul><li>**AutoScript.** O mesmo que **detecção automática**, exceto que ele também executa um script. Você deve especificar a URL do script imediatamente após a marca de uso. Por exemplo, `AUTOSCRIPT http://server/proxy.js`.</li><li>**Definido.** O mesmo que **NO_PROXY**, exceto que ele remove as URLs de proxy manuais (se especificado) e todas as URLs descobertas usando a detecção automática.</li></ul> |
| ConnectionName | Opcional. Usado com o parâmetro **/Conn** para especificar a conexão de modem a ser usada. Se você não especificar o parâmetro **/Conn** , o bits usará a conexão LAN. |

### <a name="remarks"></a>Comentários

Cada chamada sucessiva usando essa opção substitui o uso especificado anteriormente, mas não os parâmetros do uso definido anteriormente. Por exemplo, se você especificar **NO_PROXY**, **AUTODETECT**e **MANUAL_PROXY** em chamadas separadas, o bits usará o último uso fornecido, mas manterá os parâmetros do uso definido anteriormente.

## <a name="examples"></a>Exemplos

Para definir o uso de proxy para a conta LOCALSYSTEM:

```
bitsadmin /util /setieproxy localsystem AUTODETECT
```

```
bitsadmin /util /setieproxy localsystem MANUAL_PROXY proxy1,proxy2,proxy3 NULL
```

```
bitsadmin /util /setieproxy localsystem MANUAL_PROXY proxy1:80
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin util](bitsadmin-util.md)

- [comando Bitsadmin](bitsadmin.md)
