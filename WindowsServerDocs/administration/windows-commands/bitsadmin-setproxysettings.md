---
title: bitsadmin setproxysettings
description: Artigo de referência para o comando Bitsadmin setproxysettings, que define as configurações de proxy para o trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: be8edb1b-614e-4d0b-a8f8-64b4bde3e64b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a59bbb560b8c89134e81c02f99aaecebdb65ca89
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927584"
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
| uso | Define o uso do proxy, incluindo:<ul><li>**PRECONFIG.** Use os padrões do Internet Explorer do proprietário.</li><li>**NO_PROXY.** Não use um servidor proxy.</li><li>**Substituição.** Use uma lista de proxy explícita e a lista de bypass. A lista de proxy e as informações de bypass de proxy devem ser seguidas.</li><li>**Detecção automática.** Detecta automaticamente as configurações de proxy.</li></ul> |
| list | Usado quando o parâmetro *Usage* é definido como override. Deve conter uma lista delimitada por vírgulas de servidores proxy a serem usados. |
| ignorar | Usado quando o parâmetro *Usage* é definido como override. Deve conter uma lista delimitada por espaço de nomes de host ou endereços IP, ou ambos, para os quais as transferências não devem ser roteadas por meio de um proxy. Isso pode ser `<local>` referente a todos os servidores na mesma LAN. Valores de NULL podem ser usados para uma lista de bypass de proxy vazia. |

## <a name="examples"></a>Exemplos

Para definir as configurações de proxy usando as várias opções de uso para o trabalho chamado *myDownloadJob*:

```
bitsadmin /setproxysettings myDownloadJob PRECONFIG
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

- [comando Bitsadmin](bitsadmin.md)
