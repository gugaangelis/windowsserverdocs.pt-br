---
title: bitsadmin util e getieproxy
description: Tópico de referência para o comando Bitsadmin util e getieproxy, que recupera o uso de proxy para a conta de serviço específica.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6d50c7e3-f4eb-4ca5-9f0c-4ed396087db6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 576f308bc7fb9a4e448638d06621f95eebef0cd0
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82707661"
---
# <a name="bitsadmin-util-and-getieproxy"></a>bitsadmin util e getieproxy

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera o uso de proxy para a conta de serviço determinada. Esse comando mostra o valor para cada uso de proxy, não apenas o uso de proxy especificado para a conta de serviço. Para obter detalhes sobre como definir o uso de proxy para contas de serviço específicas, consulte o comando [Bitsadmin util e setieproxy](bitsadmin-util-and-setieproxy.md) .

## <a name="syntax"></a>Sintaxe

```
bitsadmin /util /getieproxy <account> [/conn <connectionname>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ---------- |
| account | Especifica a conta de serviço cujas configurações de proxy você deseja recuperar. Os valores possíveis incluem:<ul><li>LOCALSYSTEM</li><li>   NETWORKSERVICE</li><li>LocalService.</li></ul> |
| ConnectionName | Opcional. Usado com o parâmetro **/Conn** para especificar a conexão de modem a ser usada. Se você não especificar o parâmetro **/Conn** , o bits usará a conexão LAN. |

## <a name="examples"></a>Exemplos

Para exibir o uso de proxy para a conta de serviço de rede:

```
bitsadmin /util /getieproxy NETWORKSERVICE
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin util](bitsadmin-util.md)

- [comando Bitsadmin](bitsadmin.md)
