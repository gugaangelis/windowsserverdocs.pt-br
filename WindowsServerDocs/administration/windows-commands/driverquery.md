---
title: driverquery
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 92ca4b84-e4e2-405b-9f31-bf6db9f66839
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 88a59f9da9927bb923418695bc760303c0fb00b0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439481"
---
# <a name="driverquery"></a>driverquery



Permite que um administrador exibir uma lista de drivers de dispositivo instalados e suas propriedades. Se usado sem parâmetros, **driverquery** é executado no computador local.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
driverquery [/s <System> [/u [<Domain>\]<Username> [/p <Password>]]] [/fo {table | list | csv}] [/nh] [/v | /si]
```

## <a name="parameters"></a>Parâmetros

|         Parâmetro         |                                                                                                                                         Descrição                                                                                                                                          |
|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /s \<System>        |                                                                                      Especifica o nome ou endereço IP de um computador remoto. Não use barras invertidas. O padrão é o computador local.                                                                                       |
| /u [\<Domain>\]<Username> | Executa o comando com as credenciais da conta de usuário conforme especificado por *usuário* ou *domínio*\*usuário<em>. Por padrão, \* \*/s</em> \* usa as credenciais do usuário que está conectado no momento no computador que está emitindo o comando. **/u** não pode ser usado, a menos que **/s** for especificado. |
|      /p \<Password>       |                                                                           Especifica a senha da conta de usuário que é especificada na **/u** parâmetro. **/p** não pode ser usado, a menos que **/u** for especificado.                                                                            |
|        /FO {tabela         |                                                                                                                                             lista                                                                                                                                             |
|            /nh            |                                                                                      Omite a linha de cabeçalho a partir das informações exibidas de driver. Não é válido se o **/fo** parâmetro for definido como **lista**.                                                                                      |
|            /v             |                                                                                                               Exibe a saída detalhada. **/v** não é válido para drivers assinados.                                                                                                               |
|            /si            |                                                                                                                          Fornece informações sobre drivers assinados.                                                                                                                          |
|            /?             |                                                                                                                             Exibe a ajuda no prompt de comando.                                                                                                                             |

## <a name="BKMK_examples"></a>Exemplos

Para exibir uma lista de drivers de dispositivo instalados no computador local, digite:
```
driverquery 
```
Para exibir a saída em um formato de valores separados por vírgulas (CSV), digite:
```
driverquery /fo csv 
```
Para ocultar a linha de cabeçalho na saída, digite:
```
driverquery /nh 
```
Para usar o **driverquery** comando em um servidor remoto denominado **server1** usando suas credenciais atuais no computador local, digite:
```
driverquery /s server1
```
Para usar o **driverquery** comando em um servidor remoto denominado **server1** usando as credenciais da **user1** no domínio **maindom**, tipo:
```
driverquery /s server1 /u maindom\user1 /p p@ssw3d
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)