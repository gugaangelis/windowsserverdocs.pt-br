---
title: driverquery
description: Tópico de referência para o comando driverquery, que permite que um administrador exiba uma lista de drivers de dispositivo instalados e suas propriedades.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 92ca4b84-e4e2-405b-9f31-bf6db9f66839
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f4754cba8cf4cb3a5f01b0aeb0095f727a072a5c
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436931"
---
# <a name="driverquery"></a>driverquery

Permite que um administrador exiba uma lista de drivers de dispositivo instalados e suas propriedades. Se usado sem parâmetros, **DRIVERQUERY** é executado no computador local.

## <a name="syntax"></a>Sintaxe

```
driverquery [/s <system> [/u [<domain>\]<username> [/p <password>]]] [/fo {table | list | csv}] [/nh] [/v | /si]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- |------------ |
| /s`<system>` | Especifica o nome ou o endereço IP de um computador remoto. Não use barras invertidas. O padrão é o computador local. |
| /u`[<domain>]<username>` | Executa o comando com as credenciais da conta de usuário, conforme especificado pelo *usuário* ou *domínio \ nome_de_usuário*. Por padrão, */s* usa as credenciais do usuário que está conectado no momento no computador que está emitindo o comando. **/u** não pode ser usado a menos que **/s** seja especificado. |
| /p`<password>` | Especifica a senha da conta de usuário que é especificada no parâmetro **/u** . **/p** não pode ser usado a menos que **/u** seja especificado. |
| /Fo Table | Formata a saída como uma tabela. Este é o padrão. |
| /Fo List | Formata a saída como uma lista. |
| /Fo CSV | Formata a saída com valores separados por vírgula. |
| /NH | Omite a linha de cabeçalho das informações de driver exibidas. Não é válido se o parâmetro **/fo** estiver definido como **list**. |
| /v | Exibe a saída detalhada. **/v** não é válido para drivers assinados. |
| /si | Fornece informações sobre drivers assinados. |
| /? | Exibe a ajuda no prompt de comando. |

### <a name="examples"></a>Exemplos

Para exibir uma lista de drivers de dispositivo instalados no computador local, digite:

```
driverquery
```

Para exibir a saída em um formato CSV (valores separados por vírgula), digite:

```
driverquery /fo csv
```

Para ocultar a linha de cabeçalho na saída, digite:

```
driverquery /nh
```

Para usar o comando **DRIVERQUERY** em um servidor remoto chamado *Server1* usando suas credenciais atuais no computador local, digite:

```
driverquery /s server1
```

Para usar o comando **DRIVERQUERY** em um servidor remoto chamado *Server1* usando as credenciais para *user1* no domínio *maindom*, digite:

```
driverquery /s server1 /u maindom\user1 /p p@ssw3d
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)