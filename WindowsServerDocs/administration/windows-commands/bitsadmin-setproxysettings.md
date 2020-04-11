---
title: bitsadmin setproxysettings
description: Tópico de comandos do Windows para **Bitsadmin setproxysettings**, que define as configurações de proxy para o trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: be8edb1b-614e-4d0b-a8f8-64b4bde3e64b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ea92383d9bd09372d21d3c1da84db060b0a9958
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122752"
---
# <a name="bitsadmin-setproxysettings"></a>bitsadmin setproxysettings

Define as configurações de proxy para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /setproxysettings <job> <usage> [list] [bypass]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| usage | Define o uso do proxy, incluindo:<ul><li>**PRECONFIG.** Use os padrões do Internet Explorer do proprietário.</li><li>**NO_PROXY.** Não use um servidor proxy.</li><li>**Substituição.** Use uma lista de proxy explícita e a lista de bypass. A lista de proxy e as informações de bypass de proxy devem ser seguidas.</li><li>**Detecção automática.** Detecta automaticamente as configurações de proxy.</li></ul> |
| {1&gt;list&lt;1} | Usado quando o parâmetro *Usage* é definido como override. Deve conter uma lista delimitada por vírgulas de servidores proxy a serem usados. |
| Pular | Usado quando o parâmetro *Usage* é definido como override. Deve conter uma lista delimitada por espaço de nomes de host ou endereços IP, ou ambos, para os quais as transferências não devem ser roteadas por meio de um proxy. Isso pode ser `<local>` para se referir a todos os servidores na mesma LAN. Valores de NULL podem ser usados para uma lista de bypass de proxy vazia. |

## <a name="examples"></a>Exemplos

O exemplo a seguir define as configurações de proxy para o trabalho chamado *myDownloadJob*.

```
C:\>bitsadmin /setproxysettings myDownloadJob PRECONFIG
```

```
bitsadmin /setproxysettings myDownloadJob NO_PROXY
```
```
bitsadmin /setproxysettings myDownloadJob OVERRIDE proxy1:80
```

```
bitsadmin /setproxysettings myDownloadJob OVERRIDE proxy1,proxy2,proxy3 NULL
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)